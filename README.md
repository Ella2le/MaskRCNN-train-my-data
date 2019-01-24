# MaskRCNN-train-my-data
use MaskRCNN train my data
主要参考：

https://blog.csdn.net/qq_29462849/article/details/81037343#commentBox

https://blog.csdn.net/l297969586/article/details/79140840

https://www.youtube.com/watch?v=2TikTv6PWDw

## 一、配置MaskRCNN环境
### 1、安装Anaconda
https://www.anaconda.com/

官方安装指南（有啥问题直接去找比较全）

http://docs.anaconda.com/anaconda/install/

直接在终端中运行Anaconda的安装程序即可（请输入自己的安装包所在路径）

bash ~/Downloads/Anaconda3-5.3.0-Linux-x86_64.sh
### 2、在Anaconda下配置自己的MaskRCNN环境
conda create -n MaskRCNN python=3.6 pip  #新建一个名字为MaskRCNN的环境，注意python版本和自己的对应

python --version #查看python版本

activate MaskRCNN #激活刚才新建的环境

pip install -r requirement.txt  #安装必要的库
 txt里的内容

numpy
scipy
Pillow
cython
matplotlib
scikit-image
tensorflow>=1.3.0
keras>=2.0.8
opencv-python
h5py
imgaug
IPython[all]

把上面的内容复制到requirements.txt文件里，保存到当前操作的路径中即可（我就是在~/Download/下保存）

对应的版本号，如果有特殊要求请自己确认下，特别是GPU版本和CUDA对应的

https://blog.csdn.net/yeler082/article/details/80943040 可以参考该博客

### 3、克隆安装Mask RCNN Repo
git clone https://github.com/matterport/Mask_RCNN.git
### 4、安装pycocotools
git clone https://github.com/philferriere/cocoapi.git

pip install git+https://github.com/philferriere/cocoapi.git#subdirectory=pythonAPI
（PS：教程里说pycocotools需要visualc++ 2015 build tools，但是那个网址没打开，我没安装后面也成功了，有可能之前装搞c++时候装过）

### 5、下载coco预训练的权重
https://github.com/matterport/Mask RCNN/releases]

下载mask_rcnn_coco.h5

### 6、测试是否安装成功
在终端敲入jupyter notebook
![](https://github.com/Ella2le/MaskRCNN-train-my-data/blob/master/images/1.png)


在jupyter中打开，Mask_RCNN/samples/demo.ipynb

修改detection的读入自己找的一张doge的图片，图片放在Mask_RCNN/mrcnn/images 里面





测试安装成功

## 二、准备自己的数据集（labelme）
### 1、label工具的选择
参考

https://engineering.matterport.com/splash-of-color-instance-segmentation-with-mask-r-cnn-and-tensorflow-7c761e238b46

官方有一个blog推荐了一些label的工具，截图如下



附上链接：（部分需要科学上网）

VGG Image Annotator（VIA）

Labelme

Rectlabel

Labelbox

COCO UI

### 2、labelme的安装
最后按照教程

https://blog.csdn.net/qq_29462849/article/details/81037343#commentBox

https://blog.csdn.net/l297969586/article/details/79140840

选择了labelme

conda create -n labelme python=3.6 pip

conda activate labelme

pip install labelme

然后日常使用，就下面两句话（我在windows下做的label，操作是一样的）



然后操作基本就很简单了截图如下



保存后会生成一个json文件，

进入json所在文件，用labelme自带的数据转换 高亮语句



@echo off
for %%i in (*.json) do labelme_json_to_dataset "%%i"
pause
把上面语句保存到txt文件中，另存为bat文件，移到你json文件所在目录下，

再终端labelme环境激活的状态下输入

start "" "test.bat"

即可将目录下所有的json文件批量转换

最后得到如截图所示文件



**注意**

1、如果在labelme中同一类物体需要命名为类似cat_1,cat_2,cat_3......cat_20的形式，不然会把所有的mask储存到一个通道，所有的猫识别为一个整体。

2、训练要求一般是正方形，虽然程序是会主动填充白边，但是如果是自己制图的话，建议都将size设为正方形，

3、训练的时候IMAGE_MAX_DIM和IMAGE_MIN_DIM建议和自己图片一样，然后一定要能被2整除

4、我所用的版本省成本的label.png的深度已经是8位不用再额外剪裁

5、尽量在label前确定自己的图像尺寸，否则resize的时候保存的时候可能会出玄学问题，导致训练的时候维度不对

按照参考教程，保存整理为如下几个文件即可，放在Mask_RCNN目录下（你也可以去code里面修改dataset_rooot_path为自己的数据集路径）






pic是label的jpg原图，（ps：我label的png颜色比较鲜艳哎，不是一团黑，确定png的bit depth是8）

### 3、训练自己的数据
我用的是https://blog.csdn.net/qq_29462849/article/details/81037343#commentBox

里面分享大code，感谢前辈分享，让小白方便入门

但是我还是喜欢用jupyter那种实时反馈的效果，方便小白排错

一般只要修改

#number of object

#class_name

即可

其他参数可根据自己的硬件和图像尺寸等信息自行修改

部分参数需要去Mask_RCNN/mrcnn/config.py中去修改









