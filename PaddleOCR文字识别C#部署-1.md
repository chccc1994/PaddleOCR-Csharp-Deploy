
1. [PaddleOCR文字识别C#部署-1](https://blog.csdn.net/ch_ccc/article/details/113857304?spm=1001.2014.3001.5501)
2. [PaddleOCR文字识别C#部署-2](https://blog.csdn.net/ch_ccc/article/details/114386308)
3. [PaddleOCR文字识别C#部署-3](https://blog.csdn.net/ch_ccc/article/details/114573779)
`审核人审核文章带点脑子。`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210324171737288.gif#pic_center)


<center><font size=8 color=red face="行楷"> 号外  号外 最新最强更新...</font></center>

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210413101554186.gif#pic_center)


<font face ="楷体">正文：

Windows下PaddleOCR的C++编译并生成dll

##### 1. 使用CMake编译PaddleOCR C++文件生成本地化工程文件
使用CMake编译PaddleOCR C++文件生成本地化工程文件

1.1 准备工作


1）安装CMake 3.18.0，Visual Studio 2019，OpenCV 4.5.1三个软件。
2）PaddleOCR [下载](https://github.com/PaddlePaddle/PaddleOCR)。
3）根据自己的CUDA和cuDNN版本，下载相应的Paddle官方提供的[Windows预测库](https://www.paddlepaddle.org.cn/documentation/docs/zh/guides/05_inference_deployment/inference/windows_cpp_inference.html)，我用的版本是paddle_inference.zip，其他版本未测试。
4) [推理模型库](https://github.com/PaddlePaddle/PaddleOCR/blob/release/2.0/README_ch.md)准备。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210219101052251.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoX2NjYw==,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20210219101706459.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoX2NjYw==,size_16,color_FFFFFF,t_70)综上，将以上paddle OCR、预测库和模型放在同一文件夹中方便后续添加（<font color="blue">无强制要求</font>）。

1.2 CMake编译PaddleOCR
本人没有用到GPU，CUDA_LIB未填写，如需用到GPU需要填写CUDA地址。
CMake编译PaddleOCR可以参考[Windows 下 PaddleOCR C++推理部署 cmake vs2017](https://blog.csdn.net/xingtianyao/article/details/110625756)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021021910242716.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoX2NjYw==,size_16,color_FFFFFF,t_70)
##### 2. C++生成DLL文件
CMake PaddleOCR在Configuring done和Generating done后，点击Open Project，即会自动用Visual Studio 2019打开本地化工程文件。<font color=red>建议:利用原始文件先生成ocr_system.exe测试一下，CMake后本地文件可用。</font>
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210219103021895.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoX2NjYw==,size_16,color_FFFFFF,t_70)
2.1 测试ocr_system.exe
<font color=blue>右键-->仅用于项目-->仅生成ocr_system</font>， 生成ocr_system.exe，打开cmd cd到Release目录下，可能会缺失paddle_fluid.dll和opencv_world451.dll动态链接库，从opencv和推理预测库中复制。cmd运行。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210219131545891.png)
```cpp
ocr_system.exe D:\paddle\PaddleOCR\deploy\cpp_infer\tools\config.txt D:\paddle\PaddleOCR\doc\imgs\1.jpg
```
<font  color=#999aaa>
在Windows下的终端中执行文件exe时，可能会发生乱码的现象，此时需要在终端中输入 CHCP 65001 ，将终端的编码方式由GBK编码(默认)改为UTF-8编码，更加具体的解释可以参考这篇博客：https://blog.csdn.net/qq_35038153/article/details/78430359。
————————————————————


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210219131622798.png)![在这里插入图片描述](https://img-blog.csdnimg.cn/20210219132801228.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoX2NjYw==,size_16,color_FFFFFF,t_70)2.2 修改main.cpp 生成dll
右键-->属性 配置属性Release，平台x64，配置类型 动态库dll，比较坑的地方：更改完<font color =red>配置类型 动态库dll</font>后，编译生成仅生成ocr_system还是生成的exe，后续更改了<font color =red>高级</font>：目标文件扩展名。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210219133347421.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoX2NjYw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210219133941890.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoX2NjYw==,size_16,color_FFFFFF,t_70)
```cpp
#include "glog/logging.h"
#include "omp.h"
#include "opencv2/core.hpp"
#include "opencv2/imgcodecs.hpp"
#include "opencv2/imgproc.hpp"
#include <chrono>
#include <iomanip>
#include <iostream>
#include <ostream>
#include <vector>

#include <cstring>
#include <fstream>
#include <numeric>

#include <include/config.h>
#include <include/ocr_det.h>
#include <include/ocr_rec.h>
//#include"ocr.h"
using namespace std;
using namespace cv;
using namespace PaddleOCR;

/*
1.返回框选字体；
2.返回解析字符串
*/

extern "C" __declspec(dllexport) cv::Mat * LoadModel(char* input, int width, int height);
__declspec(dllexport) cv::Mat* LoadModel(char* input, int width, int height) 
{

  OCRConfig config("D:\\Paddle\\PaddleOCR\\deploy\\cpp_infer\\tools\\config.txt");

  config.PrintConfigInfo();

  //std::string img_path(argv[2]);
  //cv::Mat srcimg = cv::imread(img_path, cv::IMREAD_COLOR);
  cv::Mat srcimg(height, width, CV_8UC3, input);

  DBDetector det(config.det_model_dir, config.use_gpu, config.gpu_id,
                 config.gpu_mem, config.cpu_math_library_num_threads,
                 config.use_mkldnn, config.max_side_len, config.det_db_thresh,
                 config.det_db_box_thresh, config.det_db_unclip_ratio,
                 config.visualize, config.use_tensorrt, config.use_fp16);

  Classifier *cls = nullptr;
  if (config.use_angle_cls == true) {
    cls = new Classifier(config.cls_model_dir, config.use_gpu, config.gpu_id,
                         config.gpu_mem, config.cpu_math_library_num_threads,
                         config.use_mkldnn, config.cls_thresh,
                         config.use_tensorrt, config.use_fp16);
  }

  CRNNRecognizer rec(config.rec_model_dir, config.use_gpu, config.gpu_id,
                     config.gpu_mem, config.cpu_math_library_num_threads,
                     config.use_mkldnn, config.char_list_file,
                     config.use_tensorrt, config.use_fp16);

  auto start = std::chrono::system_clock::now();
  std::vector<std::vector<std::vector<int>>> boxes;
  // 检测
  cv::Mat ocrImage;
  ocrImage= det.Run(srcimg, boxes);
  // 识别
  rec.Run(boxes, srcimg, cls);
  auto end = std::chrono::system_clock::now();
  auto duration =
      std::chrono::duration_cast<std::chrono::microseconds>(end - start);
  std::cout << "Cost  "
            << double(duration.count()) *
                   std::chrono::microseconds::period::num /
                   std::chrono::microseconds::period::den
            << "s" << std::endl;
  cv::Mat gray;
  cv::cvtColor(srcimg, gray, COLOR_BGR2GRAY);
  return new cv::Mat(ocrImage);
}

```





##### 3. 使用C#编写界面调用DLL实现文字识别
将动态链接库<font face="Times New Roman"  color=blue>libiomp5md.dll 、mkldnn.dll、mklml.dll、ocr_system.dll、opencv_world451.dll、paddle_fluid.dll</font > 添加到c# 项目中.![在这里插入图片描述](https://img-blog.csdnimg.cn/20210219135906268.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoX2NjYw==,size_16,color_FFFFFF,t_70)
```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Runtime.InteropServices;

using OpenCvSharp;
using OpenCvSharp.XImgProc;
namespace WindowsFormsApp1
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }
        [DllImport("ocr_system.dll", EntryPoint = "LoadModel", SetLastError = true, CharSet = CharSet.Ansi)]
        static extern IntPtr LoadModel(byte[] input, int height, int width);  //out IntPtr seg_res
        private void button1_Click(object sender, EventArgs e)
        {

            string image_path = "D://Paddle//PaddleOCR//doc//imgs//1.jpg";
            Bitmap bmp = new Bitmap(image_path);
            //pictureBox1.Image = bmp;
            int stride;
            byte[] source = GetBGRValues(bmp, out stride);
            IntPtr seg_img = LoadModel(source, bmp.Width, bmp.Height);  //out seg_img
            Mat img = new Mat(seg_img);
            Bitmap seg_show = new Bitmap(img.Cols, img.Rows, (int)img.Step(), System.Drawing.Imaging.PixelFormat.Format24bppRgb, img.Data);

            pictureBox1.Image = seg_show;
        }
        // 将Btimap类转换为byte[]类函数
        public static byte[] GetBGRValues(Bitmap bmp, out int stride)
        {
            var rect = new Rectangle(0, 0, bmp.Width, bmp.Height);
            var bmpData = bmp.LockBits(rect, System.Drawing.Imaging.ImageLockMode.ReadOnly, bmp.PixelFormat);
            stride = bmpData.Stride;
            var rowBytes = bmpData.Width * Image.GetPixelFormatSize(bmp.PixelFormat) / 8;
            var imgBytes = bmp.Height * rowBytes;
            byte[] rgbValues = new byte[imgBytes];
            IntPtr ptr = bmpData.Scan0;
            for (var i = 0; i < bmp.Height; i++)
            {
                Marshal.Copy(ptr, rgbValues, i * rowBytes, rowBytes);
                ptr += bmpData.Stride;
            }
            bmp.UnlockBits(bmpData);
            return rgbValues;
        }
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021021913572858.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoX2NjYw==,size_16,color_FFFFFF,t_70)
<font face="楷体" color=red>特别注意：</font>
	<font face="楷体" color =blue>若是与工业相机连接，需要注意图像采集的通道数，<font face="Times New Roman">PaddleOCR<font face="楷体" >不支持四通道数据输入。如需使用需要将四通道转为<font face="Times New Roman">3<font face="楷体" >通道<font face="Times New Roman">RGB。
	</font>
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210226100316603.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoX2NjYw==,size_16,color_FFFFFF,t_70)



<font size=14 color= red> 
号外 号外 重点来喽！！！！</font>


* <font color=blue>全部提取C++ 识别的文字；</font>
* <font color=blue>全部提取 box框的坐标点；</font>
* <font color=blue>全部提取每个字符串的score;</font>

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210413100747478.gif)


<font face ="楷体">提醒:部署以及源代码等相关问题请微信咨询 ..... 




<font color =red>调bug勿扰![在这里插入图片描述](https://img-blog.csdnimg.cn/20210324174728789.gif#pic_center)




<font face ="楷体">参考：
1. [PaddleOCR 文字识别 c++ win10 安装使用教程](https://blog.csdn.net/qq_38836770/article/details/109548170)
2. [Windows 下 PaddleOCR C++推理部署 cmake vs2017](https://blog.csdn.net/xingtianyao/article/details/110625756)



![在这里插入图片描述](https://img-blog.csdnimg.cn/2021030508391282.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoX2NjYw==,size_16,color_FFFFFF,t_70#pic_center)



