# 昇腾学习
> {docsify-updated}

## AscendCL


在线课程[《AscendCL视频目标检测应用开发课程（ONNX YoloV5s模型）》](https://www.hiascend.com/developer/courses/detail/1795281013304832002)，在《第1章 基础概念介绍》中，13:24左右，提到：AI 开发板上，都是在 Device 中（没有 Host）,因此不需要从Host到Device的内存拷贝（或反之）。<mark>250728：待验证确认</mark>

## 视频解码

https://gitee.com/ascend/samples/tree/master/inference/modelInference/sampleYOLOV7MultiInput；（来自刘澳；250710


<hr>

## 双目摄像头使用文档

> 来自：计科24级Y同学，2025年07月

### 基础使用

本身摄像头为 USB 直插免驱, 可使用 UVC 协议调用, 配合 OpenCV 编写 C++ 代码示例如下

```html
<p>This is a paragraph</p>
<a href="//docsify.js.org/">Docsify</a>
```

```bash
echo "hello"
```

```cpp
#include <opencv2/opencv.hpp>
int main()  {
   //  创建VideoCapture对象并打开摄像头
   cv::VideoCapture  cap(0);  //  0通常是默认的摄像头
   if  (!cap.isOpened())  {
       std::cerr  <<  "无法打开摄像头"  <<  std::endl;
       return  -1;
   }

   //  获取并显示UVC属性页

   if  (!cap.set(cv::CAP_PROP_SETTINGS))  {
       std::cerr  <<  "无法打开UVC属性页"  <<  std::endl;
       return  -1;
   }

   //  在这里，UVC属性页将会被显示，用户可以手动调整曝光值
   //  用户调整完成后，关闭属性页，代码继续执行

   //  检查摄像头是否支持曝光属性
   if  (cap.get(cv::CAP_PROP_EXPOSURE)  ==  0)  {
       std::cerr  <<  "摄像头不支持曝光设置"  <<  std::endl;
       return  -1;
   }
   //  获取当前曝光值
   double  exposure  =  cap.get(cv::CAP_PROP_EXPOSURE);
   std::cout  <<  "当前曝光值:  "  <<  exposure  <<  std::endl;

   //  设置新的曝光值，例如设置为50
   cap.set(cv::CAP_PROP_EXPOSURE,  50.0);

   //  再次获取并显示新的曝光值，以确认是否设置成功
   exposure  =  cap.get(cv::CAP_PROP_EXPOSURE);
   std::cout  <<  "新的曝光值:  "  <<  exposure  <<  std::endl;
   //  释放摄像头资源
   cap.release();
   return  0;
```



在上述代码中，`cv::VideoCapture`  类用于打开摄像头，并通过调用  `set(cv::CAP_PROP_SETTINGS)`  函数来打开UVC属性页。用户可以在属性页中手动调整曝光值。

如果想要在代码中自动设置曝光值，而不是通过属性页手动设置，可以直接使用  `set(cv::CAP_PROP_EXPOSURE,  exposureValue)`  函数，其中  `exposureValue`  是预期曝光值。

### 两个摄像头的区分

USB连接上两个摄像头后, 可利用 `V4L2（Video4Linux2）` 来获取和设置摄像头的信息, 在命令行中执行

```bash
v4l2-ctl --list-devices
```

会输出

```bash
RGB CAM: RGB CAM (usb-xhci-hcd.1.auto-1.1.2):
/dev/video2
/dev/video3
/dev/media1
RGB CAM: RGB CAM (usb-xhci-hcd.1.auto-1.1.3):
/dev/video0
/dev/video1
/dev/media2
```

后续经过测试

- `/dev/video0` 为普通可见光摄像头
- `dev/video2` 为带有红外补光的摄像头 (在调用它时会自动打开)

可利用 python 代码调用

```python
def camera_open():
    #可见光
    cap = None
    device_path = "/dev/video0"
    
    if not check_camera_device(device_path):
        print(f"错误: 可见光摄像头设备 {device_path} 不存在或无法访问")
        return
    
    try:
        cap = cv2.VideoCapture(device_path)
        if not cap.isOpened():
            print(f"错误: 无法打开可见光摄像头 {device_path}")
            return
    #红外补光
    cap = None
    device_path = "/dev/video2"
    
    if not check_camera_device(device_path):
        print(f"错误: 红外补光摄像头设备 {device_path} 不存在或无法访问")
        return
    
    try:
        cap = cv2.VideoCapture(device_path)
        if not cap.isOpened():
            print(f"错误: 无法打开红外补光摄像头 {device_path}")
            return
```

另注: 如果同时调用两个摄像头, 需要注意设置的帧数要较低一些, 否则带宽不足 (经测试可同时设置20帧,若为30帧则只能调用一个摄像头)
