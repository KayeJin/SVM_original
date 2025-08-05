
# MNIST datasets
The MNIST database of handwritten digits has a training set of 60,000 examples, and a test set of 10,000 examples.
Four files are available:

- train-images-idx3-ubyte.gz: training set images (9912422 bytes)
- train-labels-idx1-ubyte.gz: training set labels (28881 bytes)
- t10k-images-idx3-ubyte.gz: test set images (1648877 bytes)
- t10k-labels-idx1-ubyte.gz: test set labels (4542 bytes)

## MNIST
MNIST 是一个手写数字图像数据集，是机器学习领域的一个经典数据集。这个数据集包含了 60000 张的训练图像和 10000 张的测试图像。

官方下载地址：[http://yann.lecun.com/exdb/mnist/](http://yann.lecun.com/exdb/mnist/)

MNIST 数据集 CSV 格式下载地址：[https://pjreddie.com/projects/mnist-in-csv/](https://pjreddie.com/projects/mnist-in-csv/)

MNIST 较小子集链接（CSV 格式）：  
100 条记录训练数据集：[https://raw.githubusercontent.com/makeyourownneuralnetwork/makeyourownneuralnetwork/master/mnist_dataset/mnist_train_100.csv](https://raw.githubusercontent.com/makeyourownneuralnetwork/makeyourownneuralnetwork/master/mnist_dataset/mnist_train_100.csv)  
10 条记录测试数据集：[https://raw.githubusercontent.com/makeyourownneuralnetwork/makeyourownneuralnetwork/master/mnist_dataset/mnist_test_10.csv](https://raw.githubusercontent.com/makeyourownneuralnetwork/makeyourownneuralnetwork/master/mnist_dataset/mnist_test_10.csv)

# IDX 格式

MNIST 文件 的格式分为`idx3-ubyte`和`idx1-ubyte`，为IDX数据格式。IDX格式为：

```C++
magic number
size in dimension 0
size in dimension 1
size in dimension 2
.....
size in dimension N
data
```

- magic number：4字节，前2字节为0，第3字节表示数据类型
```C++
0x08: unsigned byte
0x09: signed byte
0x0B: short (2 bytes)
0x0C: int (4 bytes)
0x0D: float (4 bytes)
0x0E: double (8 bytes)
```
第四字节表示维度：1 表示一维（vectors），2 表示二维（matrices），3 表示三维（numpy的图像，有高、宽、通道数）

# idx1-ubyte

- MNIST的【标签文件】的格式，表示为
```C++
[offset] [type]          [value]          [description]
0000     32 bit integer  0x00000801(2049) magic number (MSB first)
0004     32 bit integer  60000            number of items
0008     unsigned byte   ??               label
0009     unsigned byte   ??               label
........
xxxx     unsigned byte   ??               label
```

- 0 ~ 3字节，记录文件数据格式，`0x0801`表示`unsigned byte, 1-D`（？文本格式）
- 4～7个字节，取值为60000（训练集时）或10000（测试集时），用来记录标签数据的个数；
- 8个字节 ~ ，取值为对应0~9 之间的标签数字，用来记录样本的标签。

# idx3-ubyte

- MNIST的【图像文件】的格式，表示为
```c++
[offset] [type]          [value]          [description]
0000     32 bit integer  0x00000803(2051) magic number
0004     32 bit integer  60000            number of images
0008     32 bit integer  28               number of rows
0012     32 bit integer  28               number of columns
0016     unsigned byte   ??               pixel
0017     unsigned byte   ??               pixel
........
xxxx     unsigned byte   ??               pixel
```

- 0 ~ 3字节，记录文件数据格式，`0x0803`表示`unsigned byte, 3-D`，即图片
- 4～7字节，取值为60000（训练集时）或10000（测试集时），用来记录图片数据的个数；
- 8～11字节，取值为28，用来记录图片数据的<font color="#c0504d">高度</font>；
- 12～15字节，取值为28，用来记录图片数据的<font color="#c0504d">宽度</font>；
- 16个字节 ~ ，取值为<font color="#c0504d">0~255之间的灰度值</font>，用来记录图片按行展开后得到的<font color="#c0504d">灰度值数据</font>，其中0表示背景（白色），255表示前景（黑色）。

# 参考
1. https://www.cnblogs.com/gshang/p/13022239.html