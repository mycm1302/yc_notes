# ROS2하드웨어통합

> 상위: [[ROS2]]

실제 하드웨어와 ROS2 시스템 간의 통신 및 통합 방법을 다룹니다.

---

## 🔌 하드웨어 통합 개요

### 주요 통신 방식
- **Serial Communication**: UART, USB-Serial
- **CAN Bus**: Controller Area Network
- **Ethernet**: TCP/UDP, EtherCAT
- **SPI/I2C**: 저수준 디지털 통신
- **GPIO**: 디지털 입출력

### 통합 아키텍처
```
ROS2 Application Layer
    ↓
Hardware Interface Layer
    ↓
Communication Protocol
    ↓
Physical Hardware
```

---

## 📡 Serial 통신

### 기본 Serial 노드
```cpp
#include <rclcpp/rclcpp.hpp>
#include <std_msgs/msg/string.hpp>
#include <serial/serial.h>

class SerialNode : public rclcpp::Node
{
public:
  SerialNode() : Node("serial_node")
  {
    // Serial 포트 설정
    try {
      serial_port_.setPort("/dev/ttyUSB0");
      serial_port_.setBaudrate(115200);
      serial_port_.setBytesize(serial::eightbits);
      serial_port_.setParity(serial::parity_none);
      serial_port_.setStopbits(serial::stopbits_one);
      serial_port_.open();
    }
    catch (serial::IOException& e) {
      RCLCPP_ERROR(this->get_logger(), "Unable to open serial port");
      return;
    }
    
    // 퍼블리셔 및 서브스크라이버 생성
    publisher_ = this->create_publisher<std_msgs::msg::String>("serial_data", 10);
    subscription_ = this->create_subscription<std_msgs::msg::String>(
      "serial_command", 10,
      std::bind(&SerialNode::commandCallback, this, std::placeholders::_1));
    
    // 주기적 데이터 읽기
    timer_ = this->create_wall_timer(
      std::chrono::milliseconds(50),
      std::bind(&SerialNode::readData, this));
  }

private:
  void readData()
  {
    if (serial_port_.available()) {
      std::string data = serial_port_.read(serial_port_.available());
      auto message = std_msgs::msg::String();
      message.data = data;
      publisher_->publish(message);
    }
  }
  
  void commandCallback(const std_msgs::msg::String::SharedPtr msg)
  {
    serial_port_.write(msg->data + "\n");
  }
  
  serial::Serial serial_port_;
  rclcpp::Publisher<std_msgs::msg::String>::SharedPtr publisher_;
  rclcpp::Subscription<std_msgs::msg::String>::SharedPtr subscription_;
  rclcpp::TimerBase::SharedPtr timer_;
};
```

### Python Serial 인터페이스
```python
import rclpy
from rclpy.node import Node
from std_msgs.msg import String
import serial
import threading

class SerialNode(Node):
    def __init__(self):
        super().__init__('serial_node')
        
        # Serial 포트 설정
        try:
            self.serial_port = serial.Serial(
                port='/dev/ttyUSB0',
                baudrate=115200,
                timeout=1
            )
        except serial.SerialException as e:
            self.get_logger().error(f'Failed to open serial port: {e}')
            return
        
        # 퍼블리셔 및 서브스크라이버
        self.publisher_ = self.create_publisher(String, 'serial_data', 10)
        self.subscription = self.create_subscription(
            String, 'serial_command', self.command_callback, 10)
        
        # 별도 스레드에서 데이터 읽기
        self.read_thread = threading.Thread(target=self.read_serial_data)
        self.read_thread.daemon = True
        self.read_thread.start()
    
    def read_serial_data(self):
        while rclpy.ok():
            try:
                if self.serial_port.in_waiting > 0:
                    data = self.serial_port.readline().decode('utf-8').strip()
                    msg = String()
                    msg.data = data
                    self.publisher_.publish(msg)
            except Exception as e:
                self.get_logger().error(f'Serial read error: {e}')
    
    def command_callback(self, msg):
        try:
            self.serial_port.write((msg.data + '\n').encode('utf-8'))
        except Exception as e:
            self.get_logger().error(f'Serial write error: {e}')
```

---

## 🚗 CAN Bus 통신

### SocketCAN 설정
```bash
# CAN 인터페이스 설정
sudo modprobe can
sudo modprobe can_raw
sudo modprobe vcan

# Virtual CAN 생성 (테스트용)
sudo ip link add dev vcan0 type vcan
sudo ip link set up vcan0

# 실제 CAN 인터페이스 설정
sudo ip link set can0 type can bitrate 500000
sudo ip link set up can0
```

### CAN Bridge 노드
```cpp
#include <rclcpp/rclcpp.hpp>
#include <can_msgs/msg/frame.hpp>
#include <linux/can.h>
#include <linux/can/raw.h>
#include <sys/socket.h>
#include <sys/ioctl.h>
#include <net/if.h>

class CANBridge : public rclcpp::Node
{
public:
  CANBridge() : Node("can_bridge")
  {
    // CAN 소켓 생성
    can_socket_ = socket(PF_CAN, SOCK_RAW, CAN_RAW);
    if (can_socket_ < 0) {
      RCLCPP_ERROR(this->get_logger(), "Failed to create CAN socket");
      return;
    }
    
    // CAN 인터페이스 바인딩
    struct ifreq ifr;
    strcpy(ifr.ifr_name, "can0");
    ioctl(can_socket_, SIOCGIFINDEX, &ifr);
    
    struct sockaddr_can addr;
    addr.can_family = AF_CAN;
    addr.can_ifindex = ifr.ifr_ifindex;
    
    if (bind(can_socket_, (struct sockaddr*)&addr, sizeof(addr)) < 0) {
      RCLCPP_ERROR(this->get_logger(), "Failed to bind CAN socket");
      return;
    }
    
    // ROS2 퍼블리셔/서브스크라이버
    can_publisher_ = this->create_publisher<can_msgs::msg::Frame>("can_rx", 10);
    can_subscription_ = this->create_subscription<can_msgs::msg::Frame>(
      "can_tx", 10,
      std::bind(&CANBridge::canTxCallback, this, std::placeholders::_1));
    
    // CAN 수신 타이머
    timer_ = this->create_wall_timer(
      std::chrono::milliseconds(1),
      std::bind(&CANBridge::readCANFrame, this));
  }

private:
  void readCANFrame()
  {
    struct can_frame frame;
    ssize_t nbytes = read(can_socket_, &frame, sizeof(frame));
    
    if (nbytes > 0) {
      auto msg = can_msgs::msg::Frame();
      msg.id = frame.can_id;
      msg.dlc = frame.can_dlc;
      for (int i = 0; i < frame.can_dlc; i++) {
        msg.data[i] = frame.data[i];
      }
      can_publisher_->publish(msg);
    }
  }
  
  void canTxCallback(const can_msgs::msg::Frame::SharedPtr msg)
  {
    struct can_frame frame;
    frame.can_id = msg->id;
    frame.can_dlc = msg->dlc;
    for (int i = 0; i < msg->dlc; i++) {
      frame.data[i] = msg->data[i];
    }
    
    write(can_socket_, &frame, sizeof(frame));
  }
  
  int can_socket_;
  rclcpp::Publisher<can_msgs::msg::Frame>::SharedPtr can_publisher_;
  rclcpp::Subscription<can_msgs::msg::Frame>::SharedPtr can_subscription_;
  rclcpp::TimerBase::SharedPtr timer_;
};
```

---

## ⚡ EtherCAT 통합

### SOEM 기반 EtherCAT
```cpp
#include <ethercat.h>
#include <rclcpp/rclcpp.hpp>
#include <sensor_msgs/msg/joint_state.hpp>

class EtherCATNode : public rclcpp::Node
{
public:
  EtherCATNode() : Node("ethercat_node")
  {
    // EtherCAT 마스터 초기화
    if (ec_init("eth0")) {
      RCLCPP_INFO(this->get_logger(), "EtherCAT initialized on eth0");
      
      // 슬레이브 검색
      if (ec_config_init(FALSE) > 0) {
        RCLCPP_INFO(this->get_logger(), "Found %d slaves", ec_slavecount);
        
        // PDO 매핑 설정
        ec_config_map(&IOmap_);
        ec_configdc();
        
        // SAFE-OP 상태로 전환
        ec_statecheck(0, EC_STATE_SAFE_OP, EC_TIMEOUTSTATE * 4);
        
        // OP 상태로 전환
        ec_slave[0].state = EC_STATE_OPERATIONAL;
        ec_send_processdata();
        ec_receive_processdata(EC_TIMEOUTRET);
        ec_writestate(0);
        ec_statecheck(0, EC_STATE_OPERATIONAL, EC_TIMEOUTSTATE);
        
        if (ec_slave[0].state == EC_STATE_OPERATIONAL) {
          RCLCPP_INFO(this->get_logger(), "EtherCAT in operational state");
          
          // 주기적 데이터 교환
          timer_ = this->create_wall_timer(
            std::chrono::milliseconds(1),
            std::bind(&EtherCATNode::cyclicTask, this));
        }
      }
    }
    
    joint_pub_ = this->create_publisher<sensor_msgs::msg::JointState>("joint_states", 10);
  }

private:
  void cyclicTask()
  {
    ec_send_processdata();
    ec_receive_processdata(EC_TIMEOUTRET);
    
    // 조인트 상태 발행
    auto joint_msg = sensor_msgs::msg::JointState();
    joint_msg.header.stamp = this->now();
    joint_msg.name = {"joint1", "joint2", "joint3"};
    
    // EtherCAT PDO 데이터에서 위치 읽기
    // (실제 구현은 슬레이브 매뉴얼 참조)
    joint_msg.position = {0.0, 0.0, 0.0};  // 실제 PDO 데이터로 대체
    joint_msg.velocity = {0.0, 0.0, 0.0};
    joint_msg.effort = {0.0, 0.0, 0.0};
    
    joint_pub_->publish(joint_msg);
  }
  
  char IOmap_[4096];
  rclcpp::Publisher<sensor_msgs::msg::JointState>::SharedPtr joint_pub_;
  rclcpp::TimerBase::SharedPtr timer_;
};
```

---

## 🎛️ GPIO 제어

### Linux GPIO 인터페이스
```cpp
#include <rclcpp/rclcpp.hpp>
#include <std_msgs/msg/bool.hpp>
#include <fstream>
#include <string>

class GPIONode : public rclcpp::Node
{
public:
  GPIONode() : Node("gpio_node")
  {
    gpio_pin_ = 18;  // GPIO 18번 핀
    
    // GPIO 내보내기
    exportGPIO(gpio_pin_);
    setGPIODirection(gpio_pin_, "out");
    
    // 서브스크라이버 생성
    subscription_ = this->create_subscription<std_msgs::msg::Bool>(
      "gpio_command", 10,
      std::bind(&GPIONode::gpioCallback, this, std::placeholders::_1));
    
    // GPIO 상태 퍼블리셔
    publisher_ = this->create_publisher<std_msgs::msg::Bool>("gpio_status", 10);
    
    // 주기적 상태 읽기
    timer_ = this->create_wall_timer(
      std::chrono::milliseconds(100),
      std::bind(&GPIONode::readGPIOStatus, this));
  }

private:
  void exportGPIO(int pin)
  {
    std::ofstream export_file("/sys/class/gpio/export");
    export_file << pin;
    export_file.close();
  }
  
  void setGPIODirection(int pin, const std::string& direction)
  {
    std::string path = "/sys/class/gpio/gpio" + std::to_string(pin) + "/direction";
    std::ofstream direction_file(path);
    direction_file << direction;
    direction_file.close();
  }
  
  void setGPIOValue(int pin, int value)
  {
    std::string path = "/sys/class/gpio/gpio" + std::to_string(pin) + "/value";
    std::ofstream value_file(path);
    value_file << value;
    value_file.close();
  }
  
  int getGPIOValue(int pin)
  {
    std::string path = "/sys/class/gpio/gpio" + std::to_string(pin) + "/value";
    std::ifstream value_file(path);
    int value;
    value_file >> value;
    value_file.close();
    return value;
  }
  
  void gpioCallback(const std_msgs::msg::Bool::SharedPtr msg)
  {
    setGPIOValue(gpio_pin_, msg->data ? 1 : 0);
  }
  
  void readGPIOStatus()
  {
    auto msg = std_msgs::msg::Bool();
    msg.data = (getGPIOValue(gpio_pin_) == 1);
    publisher_->publish(msg);
  }
  
  int gpio_pin_;
  rclcpp::Publisher<std_msgs::msg::Bool>::SharedPtr publisher_;
  rclcpp::Subscription<std_msgs::msg::Bool>::SharedPtr subscription_;
  rclcpp::TimerBase::SharedPtr timer_;
};
```

---

## 🔌 ros2_control 하드웨어 인터페이스

### 커스텀 하드웨어 인터페이스
```cpp
#include <hardware_interface/system_interface.hpp>
#include <hardware_interface/handle.hpp>
#include <hardware_interface/hardware_info.hpp>
#include <rclcpp/rclcpp.hpp>

class CustomHardwareInterface : public hardware_interface::SystemInterface
{
public:
  hardware_interface::CallbackReturn on_init(
    const hardware_interface::HardwareInfo& info) override
  {
    if (hardware_interface::SystemInterface::on_init(info) != 
        hardware_interface::CallbackReturn::SUCCESS) {
      return hardware_interface::CallbackReturn::ERROR;
    }
    
    // 하드웨어 초기화
    initializeHardware();
    
    // 상태 및 명령 인터페이스 설정
    for (const auto& joint : info_.joints) {
      joint_position_states_.push_back(0.0);
      joint_velocity_states_.push_back(0.0);
      joint_position_commands_.push_back(0.0);
    }
    
    return hardware_interface::CallbackReturn::SUCCESS;
  }
  
  std::vector<hardware_interface::StateInterface> export_state_interfaces() override
  {
    std::vector<hardware_interface::StateInterface> state_interfaces;
    
    for (size_t i = 0; i < info_.joints.size(); i++) {
      state_interfaces.emplace_back(hardware_interface::StateInterface(
        info_.joints[i].name, hardware_interface::HW_IF_POSITION, 
        &joint_position_states_[i]));
      state_interfaces.emplace_back(hardware_interface::StateInterface(
        info_.joints[i].name, hardware_interface::HW_IF_VELOCITY, 
        &joint_velocity_states_[i]));
    }
    
    return state_interfaces;
  }
  
  std::vector<hardware_interface::CommandInterface> export_command_interfaces() override
  {
    std::vector<hardware_interface::CommandInterface> command_interfaces;
    
    for (size_t i = 0; i < info_.joints.size(); i++) {
      command_interfaces.emplace_back(hardware_interface::CommandInterface(
        info_.joints[i].name, hardware_interface::HW_IF_POSITION, 
        &joint_position_commands_[i]));
    }
    
    return command_interfaces;
  }
  
  hardware_interface::CallbackReturn on_activate(
    const rclcpp_lifecycle::State& /*previous_state*/) override
  {
    // 하드웨어 활성화
    return hardware_interface::CallbackReturn::SUCCESS;
  }
  
  hardware_interface::CallbackReturn read(
    const rclcpp::Time& /*time*/, const rclcpp::Duration& /*period*/) override
  {
    // 하드웨어에서 상태 읽기
    readHardwareStates();
    return hardware_interface::CallbackReturn::SUCCESS;
  }
  
  hardware_interface::CallbackReturn write(
    const rclcpp::Time& /*time*/, const rclcpp::Duration& /*period*/) override
  {
    // 하드웨어에 명령 쓰기
    writeHardwareCommands();
    return hardware_interface::CallbackReturn::SUCCESS;
  }

private:
  void initializeHardware()
  {
    // 실제 하드웨어 초기화 코드
    // (Serial, CAN, EtherCAT 등)
  }
  
  void readHardwareStates()
  {
    // 하드웨어에서 실제 상태 읽기
    for (size_t i = 0; i < joint_position_states_.size(); i++) {
      // joint_position_states_[i] = readActualPosition(i);
      // joint_velocity_states_[i] = readActualVelocity(i);
    }
  }
  
  void writeHardwareCommands()
  {
    // 하드웨어에 실제 명령 전송
    for (size_t i = 0; i < joint_position_commands_.size(); i++) {
      // writePositionCommand(i, joint_position_commands_[i]);
    }
  }
  
  std::vector<double> joint_position_states_;
  std::vector<double> joint_velocity_states_;
  std::vector<double> joint_position_commands_;
};
```

---

## 🔍 센서 통합

### IMU 센서 인터페이스
```cpp
#include <rclcpp/rclcpp.hpp>
#include <sensor_msgs/msg/imu.hpp>
#include <tf2/LinearMath/Quaternion.h>

class IMUNode : public rclcpp::Node
{
public:
  IMUNode() : Node("imu_node")
  {
    // IMU 하드웨어 초기화
    initializeIMU();
    
    // IMU 데이터 퍼블리셔
    imu_publisher_ = this->create_publisher<sensor_msgs::msg::Imu>("imu/data", 10);
    
    // 주기적 데이터 읽기
    timer_ = this->create_wall_timer(
      std::chrono::milliseconds(10),  // 100Hz
      std::bind(&IMUNode::publishIMUData, this));
  }

private:
  void initializeIMU()
  {
    // I2C 또는 SPI를 통한 IMU 초기화
    // 실제 구현은 IMU 칩 매뉴얼 참조
  }
  
  void publishIMUData()
  {
    auto imu_msg = sensor_msgs::msg::Imu();
    imu_msg.header.stamp = this->now();
    imu_msg.header.frame_id = "imu_link";
    
    // 가속도계 데이터 읽기
    imu_msg.linear_acceleration.x = readAccelX();
    imu_msg.linear_acceleration.y = readAccelY();
    imu_msg.linear_acceleration.z = readAccelZ();
    
    // 자이로스코프 데이터 읽기
    imu_msg.angular_velocity.x = readGyroX();
    imu_msg.angular_velocity.y = readGyroY();
    imu_msg.angular_velocity.z = readGyroZ();
    
    // 자세 계산 (간단한 예제)
    tf2::Quaternion q;
    q.setRPY(0, 0, 0);  // 실제로는 센서 융합 알고리즘 필요
    imu_msg.orientation.x = q.x();
    imu_msg.orientation.y = q.y();
    imu_msg.orientation.z = q.z();
    imu_msg.orientation.w = q.w();
    
    imu_publisher_->publish(imu_msg);
  }
  
  double readAccelX() { return 0.0; }  // 실제 하드웨어 읽기 구현
  double readAccelY() { return 0.0; }
  double readAccelZ() { return 9.81; }
  double readGyroX() { return 0.0; }
  double readGyroY() { return 0.0; }
  double readGyroZ() { return 0.0; }
  
  rclcpp::Publisher<sensor_msgs::msg::Imu>::SharedPtr imu_publisher_;
  rclcpp::TimerBase::SharedPtr timer_;
};
```

---

## 🛠️ 디버깅 및 모니터링

### 하드웨어 진단
```cpp
#include <diagnostic_msgs/msg/diagnostic_array.hpp>
#include <diagnostic_msgs/msg/diagnostic_status.hpp>

class HardwareDiagnostics : public rclcpp::Node
{
public:
  HardwareDiagnostics() : Node("hardware_diagnostics")
  {
    diagnostics_pub_ = this->create_publisher<diagnostic_msgs::msg::DiagnosticArray>(
      "/diagnostics", 10);
    
    timer_ = this->create_wall_timer(
      std::chrono::seconds(1),
      std::bind(&HardwareDiagnostics::publishDiagnostics, this));
  }

private:
  void publishDiagnostics()
  {
    auto diag_array = diagnostic_msgs::msg::DiagnosticArray();
    diag_array.header.stamp = this->now();
    
    // 하드웨어 상태 검사
    auto hw_status = diagnostic_msgs::msg::DiagnosticStatus();
    hw_status.name = "Hardware Connection";
    hw_status.hardware_id = "custom_hardware";
    
    if (checkHardwareConnection()) {
      hw_status.level = diagnostic_msgs::msg::DiagnosticStatus::OK;
      hw_status.message = "Hardware connection OK";
    } else {
      hw_status.level = diagnostic_msgs::msg::DiagnosticStatus::ERROR;
      hw_status.message = "Hardware connection failed";
    }
    
    diag_array.status.push_back(hw_status);
    diagnostics_pub_->publish(diag_array);
  }
  
  bool checkHardwareConnection()
  {
    // 실제 하드웨어 연결 상태 확인
    return true;
  }
  
  rclcpp::Publisher<diagnostic_msgs::msg::DiagnosticArray>::SharedPtr diagnostics_pub_;
  rclcpp::TimerBase::SharedPtr timer_;
};
```

---

## 📖 주요 참고문헌

1. **ros2_control Hardware Components**: https://control.ros.org/rolling/doc/ros2_control/hardware_interface/doc/hardware_components_userdoc.html
2. **Serial Communication**: https://github.com/RozaGkliva/ros2_serial
3. **CAN Bus Integration**: https://github.com/ROS4SPACE/ros2can_bridge
4. **EtherCAT with ROS2**: https://robotics.stackexchange.com/questions/108268/how-to-use-ros2-control-with-actual-hardware-communicating-via-ethercat
5. **micro-ROS**: https://micro.ros.org/docs/overview/hardware/