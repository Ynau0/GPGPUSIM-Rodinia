# GPGPU-SIM SPACK Rodinia实验



## 如何使用

### 一、SPACK安装

步骤如下：

1.安装spack
```
git clone https://github.com/spack/spack.git
#如果服务器无法直接clone，使用镜像代理
git clone https://mirror.ghproxy.com/https://github.com/spack/spack.git
```

2.在.bashrc文件中添加
```
# spack 
export SCC_SETUP_ENV=$(realpath ~/workspace/gpgpu-sim/spack/share/spack/setup-env.sh)
```

3.在terminal输入命令进入spack环境
```
#载入spack
. $SCC_SETUP_ENV

#加载编译器
spack compilers 

#找出已有编译器
spack compiler find
```
4.在spack中安装环境
```
spack install cuda@11.0.3
spack install gpgpu-sim@4.0.1
spack install rodinia@3.1
spack install gcc@7.5.0
```
5.常用的spack命令
```
#find package location
spack find -p <package>

#find package path
spack location -i <package>

#search installed packages
spack find

#search all packages
spack list
spack list <package name>

#find loaded packages
spack find --loaded
```
6.spack加载环境
```
spack load cuda@11.0.3
spack load gpgpu-sim@4.0.1
spack load rodinia@3.1
spack load gcc@7.5.0
```

### 二、gpgpu-sim

1.进入GPGPU-sim的环境
```
source $(spack location -i gpgpu-sim@4.0.1)/gpgpu-sim_distribution/setup_environment release
```
2.复制配置文件(以QV100为例)
```
cd $(spack location -i gpgpu-sim@4.0.1)/gpgpu-sim_distribution/configs
cp -r tested-cfgs ~
```
3.进入配置文件夹中进行模拟矩阵计算测试
```
cd ~/tested-cfgs/SM7_QV100
```
4.查看系统cpu信息
```
#list cpu info
lscpu

#查看系统CPU核心运行情况
top
#top后直接输入1查看每个核心占用情况
```
5.确定CPU空闲核心，进行实验
```
#预加载GPGPU-sim中的libcuda，taskset指令占用CPU的第8个核心。Gaussian是rodinia矩阵计算测试
LD_PRELOAD=$(spack location -i gpgpu-sim@4.0.1)/gpgpu-sim_distribution/lib/release/libcudart.so taskset -c 8 gaussian -f $(spack location -i rodinia)/data/gaussian/matrix3.txt > ./simulate3x3.log
```
6.查看实验结果
```
vim ./simulate3x3.log
```

### 三、实验数据
```
1. 在文件夹project1222中的log文件
2. speedtest文件夹中的result是在矩阵16x16的情况下不同显卡型号的模拟测试数据
3. QV100test和TitanVtest分别是QV100和TitanV在不同矩阵维度的测试数据
```
# GPGPU-Sim-Rodinia-test
