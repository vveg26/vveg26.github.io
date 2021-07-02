---
layout:     post
title:      第一个markdown文档编写
subtitle:   markdown编写
date:       2021-7-2
author:     veg
header-img: img/art-Anaconda-TensorFlow.jpg
catalog: true
tags:
    - ubuntu
    - TensorFlow
    - 深度学习
    - CUDA
---

# Install TensorFlow-GPU by Anaconda (conda install tensorflow-gpu)

It might be the simplest way to install Tensorflow or Tensorflow-GPU by conda install in the conda environment  
--

Nowadays, there are many tutorials that instruct how to install tensorflow or tensorflow-gpu. However, some people may feel it too complex just like me, because in those ways, you should download and install [NVIDIA drivers](https://www.nvidia.com/Download/index.aspx?lang=en-us), and then download and install [CUDA](https://developer.nvidia.com/cuda-downloads) (users need to pay attention to the version), afterwards you may sign an agreement and download cuDNN in [NVIDIA Developer](https://developer.nvidia.com/cudnn). Next, install python, and pip install tensorflow-gpu and so on. It's not esay for developer to do these, let alone it might causes some other error such as **version not match**, or **conflict between other python libraries** and so on. Moreover, if you want to [install tensorflow by compilation](https://www.tensorflow.org/install/gpu), it may take much more time.  