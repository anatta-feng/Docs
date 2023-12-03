> [Tesseract项目主页](https://github.com/tesseract-ocr/tesseract)

可以直接安装Tesseract，也可以源码编译。这里选择源码编译。

首先将远程仓库clone到本地。安装依赖

# 依赖

环境Ubuntu16.04

If they are not already installed, you need the following libraries (Ubuntu 16.04/14.04):

```
sudo apt-get install g++ # or clang++ (presumably)
sudo apt-get install autoconf automake libtool
sudo apt-get install autoconf-archive
sudo apt-get install pkg-config
sudo apt-get install libpng-dev
sudo apt-get install libjpeg8-dev
sudo apt-get install libtiff5-dev
sudo apt-get install zlib1g-dev
```

if you plan to install the training tools, you also need the following libraries:

```
sudo apt-get install libicu-dev
sudo apt-get install libpango1.0-dev
sudo apt-get install libcairo2-dev
```

还有Leptonica

```
sudo apt-get install libleptonica-dev
```

Leptonica和Tesseract的版本对应

|               |               |                                                              |
| ------------- | ------------- | ------------------------------------------------------------ |
| **Tesseract** | **Leptonica** | **Ubuntu**                                                   |
| 4.00          | 1.74.2        | [Ubuntu 18.04](https://packages.ubuntu.com/bionic/tesseract-ocr) |
| 3.05          | 1.74.0        | Must build from source                                       |
| 3.04          | 1.71          | [Ubuntu 16.04](http://packages.ubuntu.com/xenial/tesseract-ocr) |
| 3.03          | 1.70          | [Ubuntu 14.04](http://packages.ubuntu.com/trusty/tesseract-ocr) |
| 3.02          | 1.69          | Ubuntu 12.04                                                 |
| 3.01          | 1.67          |                                                              |

源自[项目README](https://github.com/tesseract-ocr/tesseract/wiki/Compiling#linux)

## Install elsewhere / without root

Tesseract can be configured to install anywhere, which makes it possible to install it without root access.

To install it in $HOME/local:

```
./autogen.sh
./configure --prefix=$HOME/local/
make
make install
```

To install it in \$HOME/local using Leptonica libraries also installed in $HOME/local:

```
./autogen.sh
LIBLEPT_HEADERSDIR=$HOME/local/include ./configure \
  --prefix=$HOME/local/ --with-extra-libraries=$HOME/local/lib
make
make install
```

## 编译报错

### Leptonica 1.74 or higher is required. 的解决办法

去Leptonica项目主页，下载对应版本源码，编译安装

### tesseract 运行报错：tesseract: error while loading shared libraries: libtesseract.so.4

需要更新运行链接的缓存

```
sudo ldconfig
```

# 下载字典

需要对应语言的字典才能识别相应的语言

[字典仓库](https://github.com/tesseract-ocr/tessdata_fast)

然后把里面的文件都复制到`/usr/local/share/tessdata/`路径下

至此，基础环境部署好了。

做个测试

识别这张图片

![](https://compass-ssl.xbox.com/assets/1d/8e/1d8e2937-bbfa-4de0-8d8c-3d11fe05179e.png?n=Halo-Wars-2_Page-Hero-Logo-768_370x210.png)

```
tesseract 3195358620.jpg out -l eng
cat out.txt
```
输出
```
DNAS TST)
THE NIGHTMARE
```
识别率超低

试试汉字

![](https://spaces.ac.cn/usr/uploads/2016/06/3195358620.jpg)

```
tesseract 3195358620.jpg out -l chi_sim
cat out.txt
```

输出

```
天猫
首发

上
```

一样超级低。

# 训练

