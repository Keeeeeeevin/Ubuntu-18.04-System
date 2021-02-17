1.1 显卡驱动
参考Ubuntu系统安装部分。

1.2 Cuda 10.0
1：Cuda是基于GPU并行架构的一套指令集，以C/C++/FORTRAN语言为基础，GPU编程。
2：下载Cuda 10.0版本：（已下载，《Software02.Anaconda_Cuda_Cudnn_Tensorflow》文件夹，官方安装说明文档为《CUDA_Installation_Guide_Linux.pdf》）
备注：网址https://developer.nvidia.com/cuda-90-download-archive

3：确认系统状态
备注：若安装过程出现问题，最好的参考资料是10.0版本的官方文档《CUDA_Installation_Guide_Linux.pdf》
1在联网状态下，更新软件库
sudo apt-get update
sudo apt-get upgrade
2安装依赖项（在编译Samples前安装）
sudo apt-get install g++ freeglut3-dev build-essential libx11-dev libxmu-dev libxi-dev libglu1-mesa libglu1-mesa-dev
3确认gcc版本（不严格）
gcc --version
4确认系统kernel版本，以及linux-headers
备注：Cuda10.0文件提出Ubuntu18.04内核需要4.15.0，但亲测Ubuntu18.04.03的5.0.0-23内核也可行
uname -r
sudo apt-get install linux-headers-$(uname -r)
5关闭nouveau（参考系统安装部分），执行下面命令，若无任何输出，则关闭nouveau成功。
lsmod | grep nouveau
4：安装Cuda 10.0
1安装Cuda 10.0
sudo chmod +x ./cuda_10.0.130_410.48_linux.run
sudo ./cuda_10.0.130_410.48_linux.run --no-opengl-libs（除不安装显卡驱动外，其余按默认选择即可）
备注：其中，410.48表示默认安装的显卡驱动版本，选择不安装。
3安装Cuda 10.0补丁（已下载，共1个，《Software02.Anaconda_Cuda_Cudnn_Tensorflow》文件夹）
sudo ./cuda_10.0.130.1_linux.run
5：设置环境变量
1打开环境变量设置文件
sudo gedit ~/.bashrc
2在bashrc文件末尾输入
export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
3source，使bashrc生效
source ~/.bashrc
6：Cuda Samples
1安装Cuda Sample到指定文件夹
cuda-install-samples-10.0.sh /home/kevin/Software/Cuda/Samples
2编译Sample
cd /home/kevin/Software/Cuda/Samples/NVIDIA_CUDA-10.0_Samples
make –j4
3测试Sample
cd /home/kevin/Software/Cuda/Samples/NVIDIA_CUDA-10.0_Samples/bin/x86_64/linux/release
sudo ./deviceQuery（若最后显示PASS，则表明Cuda安装成功）

1.3 Cudnn7.6.3
1：获取Cudnn 7.6.3，在Nvidia官网下载Cudnn 7.6.3，下载需要登录NVIDIA账号。
备注：（已下载，7.6.3版本，《Software02.Anaconda_Cuda_Cudnn_Tensorflow》文件夹）
2：安装Cudnn 7.6.3
1将Cudnn在/home/kevin/Software/Cudnn解压，解压后的文件夹名为cuda
2复制
sudo cp cuda/include/cudnn.h /usr/local/cuda/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*

1.4 Anaconda3
1：Anaconda是一个集成了众多科学计算包的python发行版，如numpy，scipy等，省去单独配置，可基于pyenv安装anaconda，也可单独安装
2：安装Anaconda3
1下载Anaconda3-2019.07-Linux-x86_64.sh文件
备注：已下载，《Software02.Anaconda_Cuda_Cudnn_Tensorflow》文件夹
2安装Anaconda3
sudo chmod +x ./Anaconda3-2019.07-Linux-x86_64.sh
sudo ./Anaconda3-2019.07-Linux-x86_64.sh
备注：需重新打开Terminal生效
3更换清华conda源，提升conda环境的下载速度
官方网址：https://mirror.tuna.tsinghua.edu.cn/help/anaconda/
按照官方网址操作即可。
4基本操作
conda create -n py37 python=3.7（创建环境）
conda activate py37（激活环境）
conda deactivate（退出环境）
conda remove -n py37 --all（删除环境）
conda list（列出当前环境信息）
conda env list（列出现有的环境）
conda list -n=py37（列出环境中的安装包）

1.5 TensorFlow
1：激活Anaconda环境
anaconda search -t conda tensorflow（看可以安装的版本，base环境中）
conda create -n tf_py37 python=3.7（创建环境）
conda activate tf_py37
2：安装GPU版本TensorFlow，1.14.0版本
conda install tensorflow-gpu=1.14.0

1.6 PyTorch+TensorBoardX
1：PyTorch是与TensorFlow分庭抗礼的框架
2：新创建一个Anaconda环境并激活
conda create -n torch_py37 python=3.7（创建环境）
conda activate torch_py37
3：安装PyTorch
备注：官方网址：https://pytorch.org，有不同Python，Cuda版本的安装说明。例如：利用Conda，在Python3.7及CUDA10.0版本下安装PyTorch的命令为：
conda install pytorch torchvision cudatoolkit=10.1 –c pytorch（当前为1.3版本的PyTorch，查看更新）

4：安装TensorBoardX
备注：参考官方网址：https://anaconda.org/conda-forge/tensorboardx，安装命令为：
conda install -c conda-forge tensorboardx

1.7 Docker、OpenAI-gym/Universe
1：OpenAI-Gym安装方式（足够）
git clone https://github.com/openai/gym
cd /home/kevin/Software/gym
conda activate tf_py37（或者conda activate torch_py37）
pip install -e ‘.[all]’
备注：若没提前安装MuJoCo，需要注释setup.py文件中和MuJoCo有关的安装选项（共两行）
# 'mujoco': ['mujoco_py>=1.50', 'imageio']
# 'robotics': ['mujoco_py>=1.50', 'imageio']
2：最新版本的gym和universe不兼容，如果需要universe则按照下面版本安装（暂不需要）
1  激活环境
conda activate tf_py37
2  安装Docker：https://docs.docker.com/install/linux/docker-ce/ubuntu/
3  安装OpenAI-Gym
pip install gym==0.9.5（以支持universe）
4  安装OpenAI-Universe：https://github.com/openai/universe
cd universe
pip install -e .

1.8 PyCharm
1：下载PyCharm（已下载，《Software04.IDE》文件夹）
2：安装Pycharm
1  退出Anaconda环境
conda deactivate
2  将Pycharm在home/kevin/Software/Pycharm文件夹解压
3  安装
cd /home/kevin/Software/Pycharm/Pycharm-community-2019.2.2/bin
sudo ./pycharm.sh
3：设置解释器
1  选择右下角ConfigureSettingsProject InterpreterAddExisting environment
2  选择解释器地址为：/home/kevin/.conda/envs/torch_py37/bin/python3（在该环境下安装了tensorflow）
3  选择：make available to all projects
4：新建Project时，需要选择用现有的解释器
