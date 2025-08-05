# 用昇腾NPU解码视频流 <!-- {docsify-ignore-all} -->

参考文档来自[昇腾社区首页](https://www.hiascend.com/zh)，然后选择 `开发者 | 文档`，再选择 [Atlas 200I DK A2开发者套件](https://www.hiascend.com/document/detail/zh/Atlas200IDKA2DeveloperKit/23.0.RC2/index/index.html)。

大致浏览下 “AscendCL 应用开发指南（C&C++）”。然后执行样例看看结果先。

## 运行样例

选择 `Atlas 200I DK A2开发者套件 > AscendCL 应用开发指南（C&C++） > 应用样例参考 > 媒体数据处理V2（VDEC视频解码）`[链接](https://www.hiascend.com/document/detail/zh/Atlas200IDKA2DeveloperKit/23.0.RC2/Application%20Development%20Guide/aadgc/aclcppdevg_04_0070.html)

源代码 & 待解码视频，可以参考 gitee 之 [DVPP中的VDEC功能模块，实现H264码流格式的视频解码](https://gitee.com/ascend/samples/tree/master/cplusplus/level1_single_api/7_dvpp/vdec_sample).

gitee 的 readme 写得很完整，看似要完成很多操作后，才能执行样例。其实不需要做很多操作，因为编译脚本 CMakeLists.txt 写得很完整（做了很多判断，比如环境变量没有设置就怎么怎么），因此可跳过 export 设置环境变量、chmod增加文件可执行属性等。主要就是直接编译和执行。

直接编译：
```bash
mkdir build
cd build
cmake .. -DCMAKE_CXX_COMPILER=g++ -DCMAKE_SKIP_RPATH=TRUE
make
```
屏幕显示信息如下：
```
-- The C compiler identification is GNU 11.3.0
-- The CXX compiler identification is GNU 11.3.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/g++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- env INC_PATH: /usr/local/Ascend/ascend-toolkit/latest
-- env LIB_PATH: /usr/local/Ascend/ascend-toolkit/latest/runtime/lib64/stub
-- Configuring done
-- Generating done
-- Build files have been written to: /root/samples/cplusplus/level1_single_api/7_dvpp/vdec_sample/build
```

然后直接执行即可（以“单路H264码流解码”为例）:
```bash
./vdec_demo --in_image_file ./dvpp_vdec_h264_1frame_bp_51_1920x1080.h264 --img_width 1920 --img_height 1080 --in_format 0 --in_bitwidth 8 --chn_num 1 --out_width 1920 --out_height 1080 --width_stride 1920 --height_stride 1080 --out_format 0 --out_image_file ./yuv_1920x1080_1frames_h264 --ref_frame_num 5 --dis_frame_num 3 --write_file 1
```

屏幕显示信息如下：
```
/************************Vdec Parameter********************/
InputFileName: ./dvpp_vdec_h264_1frame_bp_51_1920x1080.h264 
OutputFileName: ./yuv_1920x1080_1frames_h264 
Width: 1920 
Height: 1080 
InFormat: 0 
Bitwidth: 8 
OutWidth: 1920 
OutHeight: 1080 
OutFormat: 0 
OutWidthStride: 1920 
OutHeightStride: 1080 
ChnNum: 1 
RefFrameNum: 5 
DisplayFrameNum: 3 
OutPutOrder: 0 
SendTimes: 1 
SendInterval: 0 
DelayTime: 1 
AllocNum: 20 
StartChnNum: 0 
Render: 0 
DeviceId: 0 
/**********************************************************/
[hi_dvpp_init][1318] aclInit Success.
[hi_dvpp_init][1326] aclrtSetDevice 0 Success.
[hi_dvpp_init][1335] aclrtCreateContext Success
[hi_dvpp_init][1366] Dvpp system init success
[send_stream][672] Chn 0 send_stream Thread Exit 
[get_pic][713] Chn 0 GetFrame Success, Decode Success[1] 
[save_yuv_file][110] Chn 0, addr = e80005800200 
[save_yuv_file][111] Chn 0, Width = 1920 
[save_yuv_file][112] Chn 0, Height = 1080 
[save_yuv_file][113] Chn 0, PixelFormat = 1 
[save_yuv_file][114] Chn 0, OutWidthStride = 1920 
[save_yuv_file][115] Chn 0, OutHeightStride = 1080 
[get_pic][782] Chn 0 get_pic Thread Exit 
 --------------------------------------------------------------------------
 chnId 0, actualFrameRate 97.1, SendFrameCnt 1, DiffTime 10301 
 --------------------------------------------------------------------------
 ChanNum = 1, Average FrameRate = 97.1 fps 
[hi_dvpp_deinit][1394] Dvpp system exit success
```

生成的输出文件 yuv_1920x1080_1frames_h264_chn0_1，可以用 ffmpeg 转格式后查看，或者安装相关软件查看，此处从略。画面是雪山湖泊。

“16路H264码流解码+输出RGB888格式图片”。执行命令后，似乎要好几秒后才能输出。待有空排查下。


