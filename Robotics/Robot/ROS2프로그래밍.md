# ROS2프로그래밍

> 상위: [[ROS2]]

C++/Python APIs와 rclcpp/rclpy를 활용한 실용적인 ROS2 프로그래밍을 다룹니다.

---

## 🔧 rclcpp (C++ Client Library)

### 기본 노드 구조
```cpp
#include "rclcpp/rclcpp.hpp"

class MyNode : public rclcpp::Node
{
public:
  MyNode() : Node("my_node")
  {
    RCLCPP_INFO(this->get_logger(), "Node started");
  }
};

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<MyNode>());
  rclcpp::shutdown();
  return 0;
}
```

### Publisher/Subscriber
```cpp
// Publisher
auto publisher_ = this->create_publisher<std_msgs::msg::String>("topic", 10);
auto message = std_msgs::msg::String();
message.data = "Hello World";
publisher_->publish(message);

// Subscriber
auto subscription_ = this->create_subscription<std_msgs::msg::String>(
  "topic", 10, [this](const std_msgs::msg::String::SharedPtr msg) {
    RCLCPP_INFO(this->get_logger(), "Received: '%s'", msg->data.c_str());
  });
```

---

## 🐍 rclpy (Python Client Library)

### 기본 노드 구조
```python
import rclpy
from rclpy.node import Node

class MyNode(Node):
    def __init__(self):
        super().__init__('my_node')
        self.get_logger().info('Node started')

def main():
    rclpy.init()
    node = MyNode()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

### Publisher/Subscriber
```python
# Publisher
self.publisher_ = self.create_publisher(String, 'topic', 10)
msg = String()
msg.data = 'Hello World'
self.publisher_.publish(msg)

# Subscriber
self.subscription = self.create_subscription(
    String, 'topic', self.callback, 10)

def callback(self, msg):
    self.get_logger().info(f'Received: {msg.data}')
```

---

## ⚙️ Services & Actions

### Service 구현 (C++)
```cpp
// Service Server
auto service = this->create_service<example_interfaces::srv::AddTwoInts>(
  "add_two_ints",
  [this](const std::shared_ptr<example_interfaces::srv::AddTwoInts::Request> request,
         std::shared_ptr<example_interfaces::srv::AddTwoInts::Response> response) {
    response->sum = request->a + request->b;
  });

// Service Client
auto client = this->create_client<example_interfaces::srv::AddTwoInts>("add_two_ints");
auto request = std::make_shared<example_interfaces::srv::AddTwoInts::Request>();
request->a = 1; request->b = 2;
auto future = client->async_send_request(request);
```

### Action 구현 (Python)
```python
# Action Server
from rclpy_action import ActionServer
from action_tutorials_interfaces.action import Fibonacci

self._action_server = ActionServer(
    self, Fibonacci, 'fibonacci', self.execute_callback)

async def execute_callback(self, goal_handle):
    # Action 실행 로직
    goal_handle.succeed()
    return result
```

---

## 🔄 파라미터 관리

### 파라미터 선언 및 사용
```cpp
// C++
this->declare_parameter("my_param", 42);
int value = this->get_parameter("my_param").as_int();
```

```python
# Python  
self.declare_parameter('my_param', 42)
value = self.get_parameter('my_param').get_parameter_value().integer_value
```

### 동적 파라미터 변경
```cpp
// Parameter callback
auto param_callback = [this](const std::vector<rclcpp::Parameter> & params) {
  for (const auto & param : params) {
    if (param.get_name() == "my_param") {
      // 파라미터 변경 처리
    }
  }
  return rcl_interfaces::msg::SetParametersResult();
};
param_callback_handle_ = this->add_on_set_parameters_callback(param_callback);
```

---

## ⏰ 타이머 & 스레딩

### 타이머 사용
```cpp
// C++
auto timer_ = this->create_wall_timer(
  std::chrono::milliseconds(100),
  [this]() { this->timer_callback(); });
```

```python
# Python
self.timer = self.create_timer(0.1, self.timer_callback)
```

### 멀티스레딩
```cpp
// Callback Group
auto callback_group = this->create_callback_group(
  rclcpp::CallbackGroupType::Reentrant);

// MultiThreaded Executor
rclcpp::executors::MultiThreadedExecutor executor;
executor.add_node(node);
executor.spin();
```

---

## 🧪 테스트 작성

### 단위 테스트 (C++)
```cpp
#include <gtest/gtest.h>
#include "my_package/my_node.hpp"

TEST(MyNodeTest, BasicTest) {
  rclcpp::init(0, nullptr);
  auto node = std::make_shared<MyNode>();
  EXPECT_TRUE(node != nullptr);
  rclcpp::shutdown();
}
```

### 통합 테스트 (Python)
```python
import pytest
import rclpy
from my_package.my_node import MyNode

def test_node_creation():
    rclpy.init()
    node = MyNode()
    assert node.get_name() == 'my_node'
    node.destroy_node()
    rclpy.shutdown()
```

---

## 🔍 디버깅 & 로깅

### 로깅 시스템
```cpp
// C++ 로깅
RCLCPP_DEBUG(this->get_logger(), "Debug message");
RCLCPP_INFO(this->get_logger(), "Info message");
RCLCPP_WARN(this->get_logger(), "Warning message");
RCLCPP_ERROR(this->get_logger(), "Error message");
```

```python
# Python 로깅
self.get_logger().debug('Debug message')
self.get_logger().info('Info message')
self.get_logger().warn('Warning message')
self.get_logger().error('Error message')
```

### 디버깅 도구
- **gdb**: C++ 디버깅
- **pdb**: Python 디버깅
- **ros2 topic echo**: 메시지 확인
- **rqt_console**: 로그 GUI 뷰어

---

## 🚀 성능 최적화

### 메모리 최적화
```cpp
// 메시지 사전 할당
auto message = std::make_shared<std_msgs::msg::String>();
// 재사용을 위한 멤버 변수로 보관

// Zero-copy 전송 (지원되는 경우)
auto publisher = this->create_publisher<MyMessage>("topic", 10);
auto message = publisher->borrow_loaned_message();
// 메시지 설정
publisher->publish(std::move(message));
```

### QoS 최적화
```cpp
// 센서 데이터용 QoS
auto qos = rclcpp::SensorDataQoS();

// 시스템 기본 QoS
auto qos = rclcpp::SystemDefaultsQoS();

// 커스텀 QoS
auto qos = rclcpp::QoS(10)
  .reliability(RMW_QOS_POLICY_RELIABILITY_BEST_EFFORT)
  .history(RMW_QOS_POLICY_HISTORY_KEEP_LAST);
```

---

## 📖 주요 참고문헌

1. **rclcpp API**: https://docs.ros2.org/latest/api/rclcpp/
2. **rclpy API**: https://docs.ros2.org/latest/api/rclpy/
3. **ROS2 Programming**: ROS2 프로그래밍 실무 가이드
4. **Performance Guide**: ROS2 성능 최적화 가이드