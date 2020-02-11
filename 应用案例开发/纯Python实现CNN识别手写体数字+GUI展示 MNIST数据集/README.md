# 纯Python实现CNN识别手写体数字+GUI

> [B站视频](https://www.bilibili.com/video/av87295309?t=45)
> [GITHUB源码](https://github.com/hamlinzheng/mnist)

![](https://img.hamlinzheng.com/i/2020/02/07/psh0gw.png)



项目文件结构如下所示

```
.
├── common
│   ├── functions.py
│   ├── gradient.py
│   ├── layers.py
│   ├── optimizer.py
│   ├── trainer.py
│   └── util.py
├── dataset
│   ├── mnist.pkl
│   ├── mnist.py
│   ├── t10k-images-idx3-ubyte.gz
│   ├── t10k-labels-idx1-ubyte.gz
│   ├── train-images-idx3-ubyte.gz
│   └── train-labels-idx1-ubyte.gz
├── deep_convnet_params.pkl
├── deep_convnet.py
├── mnist_cnn_gui_main.py
├── params.pkl
├── qt
│   ├── layout.py
│   ├── layout.ui
│   ├── paintboard.py
│   └── ui2py.sh
├── simple_convnet.py
├── train_convnet.py
└── train_deepnet.py
```
