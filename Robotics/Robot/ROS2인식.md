# ROS2인식

> 상위: [[ROS2]]

컴퓨터 비전과 센서 융합을 통한 ROS2 기반 인식 시스템 구축을 다룹니다.

---

## 👁️ ROS2 인식 개요

### 핵심 기능
- **Computer Vision**: OpenCV 기반 영상 처리
- **3D Perception**: Point Cloud 처리 및 분석
- **Object Detection**: 객체 인식 및 분류
- **Sensor Fusion**: 다중 센서 통합

### 주요 패키지
- **vision_opencv**: OpenCV-ROS2 인터페이스
- **cv_bridge**: 이미지 메시지 변환
- **image_pipeline**: 이미지 처리 파이프라인
- **perception_pcl**: Point Cloud Library 통합

---

## 🌉 cv_bridge 사용

### 이미지 메시지 변환
```cpp
#include <cv_bridge/cv_bridge.h>
#include <opencv2/opencv.hpp>

// ROS2 이미지 → OpenCV Mat
void imageCallback(const sensor_msgs::msg::Image::SharedPtr msg)
{
  try {
    cv_bridge::CvImagePtr cv_ptr = cv_bridge::toCvCopy(msg, "bgr8");
    cv::Mat image = cv_ptr->image;
    
    // OpenCV 처리
    cv::Mat processed_image;
    cv::GaussianBlur(image, processed_image, cv::Size(15, 15), 0);
    
    // OpenCV Mat → ROS2 이미지
    sensor_msgs::msg::Image::SharedPtr output_msg = 
      cv_bridge::CvImage(msg->header, "bgr8", processed_image).toImageMsg();
    
    publisher_->publish(*output_msg);
  }
  catch (cv_bridge::Exception& e) {
    RCLCPP_ERROR(this->get_logger(), "cv_bridge exception: %s", e.what());
  }
}
```

### Python cv_bridge
```python
import cv2
from cv_bridge import CvBridge
import numpy as np

class ImageProcessor(Node):
    def __init__(self):
        super().__init__('image_processor')
        self.bridge = CvBridge()
        
        self.subscription = self.create_subscription(
            Image, '/camera/image_raw', self.image_callback, 10)
        self.publisher = self.create_publisher(Image, '/processed_image', 10)
    
    def image_callback(self, msg):
        try:
            cv_image = self.bridge.imgmsg_to_cv2(msg, "bgr8")
            
            # OpenCV 처리
            processed = cv2.GaussianBlur(cv_image, (15, 15), 0)
            
            # ROS2 메시지로 변환
            output_msg = self.bridge.cv2_to_imgmsg(processed, "bgr8")
            output_msg.header = msg.header
            
            self.publisher.publish(output_msg)
        except Exception as e:
            self.get_logger().error(f'Error: {e}')
```

---

## 📷 카메라 인터페이스

### USB 카메라 사용
```bash
# USB 카메라 노드 실행
ros2 run usb_cam usb_cam_node_exe

# 카메라 설정
ros2 param set /usb_cam image_width 640
ros2 param set /usb_cam image_height 480
ros2 param set /usb_cam framerate 30
```

### RealSense 카메라
```bash
# RealSense 노드 실행
ros2 launch realsense2_camera rs_launch.py

# 깊이 카메라 활성화
ros2 launch realsense2_camera rs_launch.py \
  enable_depth:=true \
  enable_color:=true \
  enable_pointcloud:=true
```

### 카메라 캘리브레이션
```bash
# 카메라 캘리브레이션 실행
ros2 run camera_calibration cameracalibrator \
  --size 8x6 \
  --square 0.108 \
  image:=/camera/image_raw \
  camera:=/camera
```

---

## 🔍 객체 인식

### 컬러 기반 객체 검출
```cpp
void detectColoredObjects(const cv::Mat& image, cv::Mat& output)
{
  cv::Mat hsv_image;
  cv::cvtColor(image, hsv_image, cv::COLOR_BGR2HSV);
  
  // 빨간색 객체 검출 (HSV 범위)
  cv::Scalar lower_red1(0, 50, 50);
  cv::Scalar upper_red1(10, 255, 255);
  cv::Scalar lower_red2(170, 50, 50);
  cv::Scalar upper_red2(180, 255, 255);
  
  cv::Mat mask1, mask2, red_mask;
  cv::inRange(hsv_image, lower_red1, upper_red1, mask1);
  cv::inRange(hsv_image, lower_red2, upper_red2, mask2);
  cv::bitwise_or(mask1, mask2, red_mask);
  
  // 컨투어 찾기
  std::vector<std::vector<cv::Point>> contours;
  cv::findContours(red_mask, contours, cv::RETR_EXTERNAL, cv::CHAIN_APPROX_SIMPLE);
  
  // 바운딩 박스 그리기
  output = image.clone();
  for (const auto& contour : contours) {
    if (cv::contourArea(contour) > 500) {  // 최소 크기 필터
      cv::Rect bbox = cv::boundingRect(contour);
      cv::rectangle(output, bbox, cv::Scalar(0, 255, 0), 2);
    }
  }
}
```

### ArUco 마커 검출
```cpp
#include <opencv2/aruco.hpp>

void detectArUcoMarkers(const cv::Mat& image, cv::Mat& output)
{
  std::vector<int> markerIds;
  std::vector<std::vector<cv::Point2f>> markerCorners;
  
  cv::Ptr<cv::aruco::Dictionary> dictionary = 
    cv::aruco::getPredefinedDictionary(cv::aruco::DICT_6X6_250);
  
  cv::aruco::detectMarkers(image, dictionary, markerCorners, markerIds);
  
  output = image.clone();
  if (markerIds.size() > 0) {
    cv::aruco::drawDetectedMarkers(output, markerCorners, markerIds);
  }
}
```

---

## ☁️ Point Cloud 처리

### PCL 통합
```cpp
#include <pcl/point_cloud.h>
#include <pcl/point_types.h>
#include <pcl_conversions/pcl_conversions.h>
#include <pcl/filters/voxel_grid.h>

void pointCloudCallback(const sensor_msgs::msg::PointCloud2::SharedPtr msg)
{
  // ROS2 PointCloud2 → PCL PointCloud
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud(new pcl::PointCloud<pcl::PointXYZ>);
  pcl::fromROSMsg(*msg, *cloud);
  
  // Voxel Grid 필터링
  pcl::PointCloud<pcl::PointXYZ>::Ptr filtered_cloud(new pcl::PointCloud<pcl::PointXYZ>);
  pcl::VoxelGrid<pcl::PointXYZ> voxel_filter;
  voxel_filter.setInputCloud(cloud);
  voxel_filter.setLeafSize(0.01f, 0.01f, 0.01f);  // 1cm 복셀
  voxel_filter.filter(*filtered_cloud);
  
  // PCL PointCloud → ROS2 PointCloud2
  sensor_msgs::msg::PointCloud2 output_msg;
  pcl::toROSMsg(*filtered_cloud, output_msg);
  output_msg.header = msg->header;
  
  publisher_->publish(output_msg);
}
```

### 평면 검출 (RANSAC)
```cpp
#include <pcl/segmentation/sac_segmentation.h>

void detectPlane(const pcl::PointCloud<pcl::PointXYZ>::Ptr& cloud)
{
  pcl::ModelCoefficients::Ptr coefficients(new pcl::ModelCoefficients);
  pcl::PointIndices::Ptr inliers(new pcl::PointIndices);
  
  pcl::SACSegmentation<pcl::PointXYZ> seg;
  seg.setOptimizeCoefficients(true);
  seg.setModelType(pcl::SACMODEL_PLANE);
  seg.setMethodType(pcl::SAC_RANSAC);
  seg.setDistanceThreshold(0.01);  // 1cm 임계값
  
  seg.setInputCloud(cloud);
  seg.segment(*inliers, *coefficients);
  
  if (inliers->indices.size() > 0) {
    RCLCPP_INFO(rclcpp::get_logger("perception"), 
                "Plane detected with %zu inliers", inliers->indices.size());
  }
}
```

---

## 🏷️ 표준 메시지 타입

### vision_msgs 사용
```cpp
#include <vision_msgs/msg/detection2_d_array.hpp>
#include <vision_msgs/msg/detection2_d.hpp>

void publishDetections(const std::vector<cv::Rect>& bboxes, 
                      const std::vector<std::string>& labels)
{
  vision_msgs::msg::Detection2DArray detection_array;
  detection_array.header.stamp = this->now();
  detection_array.header.frame_id = "camera_frame";
  
  for (size_t i = 0; i < bboxes.size(); ++i) {
    vision_msgs::msg::Detection2D detection;
    
    // 바운딩 박스 설정
    detection.bbox.center.x = bboxes[i].x + bboxes[i].width / 2.0;
    detection.bbox.center.y = bboxes[i].y + bboxes[i].height / 2.0;
    detection.bbox.size_x = bboxes[i].width;
    detection.bbox.size_y = bboxes[i].height;
    
    // 객체 가설 추가
    vision_msgs::msg::ObjectHypothesisWithPose hypothesis;
    hypothesis.hypothesis.class_id = labels[i];
    hypothesis.hypothesis.score = 0.9;  // 신뢰도
    detection.results.push_back(hypothesis);
    
    detection_array.detections.push_back(detection);
  }
  
  detection_publisher_->publish(detection_array);
}
```

---

## 🤖 AI/ML 통합

### YOLO 객체 검출
```python
import cv2
import numpy as np
from ultralytics import YOLO

class YOLODetector(Node):
    def __init__(self):
        super().__init__('yolo_detector')
        self.model = YOLO('yolov8n.pt')  # YOLOv8 nano 모델
        
        self.subscription = self.create_subscription(
            Image, '/camera/image_raw', self.image_callback, 10)
        self.publisher = self.create_publisher(
            Detection2DArray, '/detections', 10)
    
    def image_callback(self, msg):
        cv_image = self.bridge.imgmsg_to_cv2(msg, "bgr8")
        
        # YOLO 추론
        results = self.model(cv_image)
        
        # 검출 결과 처리
        detection_array = Detection2DArray()
        detection_array.header = msg.header
        
        for result in results:
            boxes = result.boxes
            if boxes is not None:
                for box in boxes:
                    detection = Detection2D()
                    # 바운딩 박스 및 클래스 정보 설정
                    # ... 세부 구현
                    detection_array.detections.append(detection)
        
        self.publisher.publish(detection_array)
```

---

## 🔄 센서 융합

### 카메라-라이다 융합
```cpp
#include <message_filters/subscriber.h>
#include <message_filters/sync_policies/approximate_time.h>

class SensorFusion : public rclcpp::Node
{
  typedef message_filters::sync_policies::ApproximateTime<
    sensor_msgs::msg::Image, 
    sensor_msgs::msg::PointCloud2> SyncPolicy;

public:
  SensorFusion() : Node("sensor_fusion")
  {
    image_sub_.subscribe(this, "/camera/image_raw");
    pointcloud_sub_.subscribe(this, "/lidar/points");
    
    sync_ = std::make_shared<message_filters::Synchronizer<SyncPolicy>>(
      SyncPolicy(10), image_sub_, pointcloud_sub_);
    sync_->registerCallback(
      std::bind(&SensorFusion::syncCallback, this, 
                std::placeholders::_1, std::placeholders::_2));
  }

private:
  void syncCallback(const sensor_msgs::msg::Image::ConstSharedPtr& image_msg,
                   const sensor_msgs::msg::PointCloud2::ConstSharedPtr& cloud_msg)
  {
    // 이미지와 포인트 클라우드 동기화 처리
    // 3D 객체 검출 및 위치 추정
  }

  message_filters::Subscriber<sensor_msgs::msg::Image> image_sub_;
  message_filters::Subscriber<sensor_msgs::msg::PointCloud2> pointcloud_sub_;
  std::shared_ptr<message_filters::Synchronizer<SyncPolicy>> sync_;
};
```

---

## 📊 시각화 및 디버깅

### RViz2 플러그인
- **Image View**: 이미지 표시
- **PointCloud2**: 포인트 클라우드 시각화
- **MarkerArray**: 검출 결과 표시
- **TF**: 좌표계 변환 표시

### rqt 도구
```bash
# 이미지 뷰어
rqt_image_view

# 그래프 플롯터
rqt_plot /detection_confidence

# 동적 재설정
rqt_reconfigure
```

---

## 🔧 성능 최적화

### GPU 가속
```cpp
// OpenCV GPU 모듈 사용
#include <opencv2/cudaimgproc.hpp>

void processWithGPU(const cv::Mat& input, cv::Mat& output)
{
  cv::cuda::GpuMat gpu_input, gpu_output;
  gpu_input.upload(input);
  
  // GPU에서 가우시안 블러 처리
  cv::cuda::GaussianBlur(gpu_input, gpu_output, cv::Size(15, 15), 0);
  
  gpu_output.download(output);
}
```

### 멀티스레딩
```cpp
// 비동기 처리
std::future<cv::Mat> processAsync(const cv::Mat& image) {
  return std::async(std::launch::async, [image]() {
    cv::Mat result;
    // 시간 소모적인 처리
    return result;
  });
}
```

---

## 📖 주요 참고문헌

1. **vision_opencv**: https://github.com/ros-perception/vision_opencv
2. **vision_msgs**: https://github.com/ros-perception/vision_msgs
3. **OpenCV Documentation**: https://docs.opencv.org/
4. **PCL Tutorials**: https://pcl.readthedocs.io/
5. **ROS2 Perception Tutorial**: https://ibrahimmansur4.medium.com/integrating-opencv-with-ros2-a-comprehensive-guide-to-computer-vision-in-robotics-66b97fa2de92