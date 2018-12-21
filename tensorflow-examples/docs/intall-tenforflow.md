# 安装 Tensorflow

## 使用 virtualenv 安装
```bash
# 在 Linux 上:
$ sudo apt-get install python-pip python-dev python-virtualenv

# 在 Mac 上:
$ sudo easy_install pip  # 如果还没有安装 pip
$ sudo pip install --upgrade virtualenv
```
1. 建立一个全新的 virtualenv 环境. 将环境建在 ~/tensorflow 目录下
```bash
virtualenv --system-site-packages ~/tensorflow
cd ~/tensorflow
# 激活 virtualenv
source bin/activate
# 激活后终端提示符会变化

# 在 virtualenv 中安装 Tensorflow
pip install --upgrade https://storage.googleapis.com/tensorflow/mac/tensorflow-0.6.0-py2-none-any.whl

# 当使用完 TensorFlow
deactivate  # 停用 virtualenv
```