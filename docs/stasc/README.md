# 昇腾学习笔记

## 用昇腾NPU解码视频流

昇腾开发板（Atlas 200I DK A2）号称可以同时解码 20路 1080P 30FPS 的视频流，因此想试试。一来可以用于真实的项目，比如接入多路监控视频流，解码后做推理，可以实现小区电梯异常行为监控。二来是进一步学习 AscendCL（Ascend Computing Language），学会通过 AscendCL 充分使用昇腾计算资源，实现视频/图像解码等处理，以及用 AscendCL 做模型推理。

详见[用昇腾NPU解码视频流](/stasc/vdec)