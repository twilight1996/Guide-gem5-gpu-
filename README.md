# Guide-gem5-gpu
#Written by Atiyeh Gheibi-Fetrat and João Dinis Ferreira
#Guide for installing gem5gpu

1_git clone https://github.com/gem5-gpu/base.git
cd base
sh ./setup_repos.sh

cd gpgpu-sim
echo "export GPGPU_SIM=`pwd`" >> /home/.bashrc
cd ..

3_sudo apt-get install build-essential
4_sudo apt-get install scons
5_sudo apt-get install python-dev
6_sudo apt-get install swig
7_sudo apt-get install libprotobuf-dev python-protobuf protobuf-compiler libgoogle-perftools-dev
8_sudo apt-get install swig gcc m4 python python-dev libgoogle-perftools-dev mercurial scons g++ build-essential

2_apt install gcc-5 g++-5
update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 10 --slave /usr/bin/g++ g++ /usr/bin/g++-5
gcc --version
g++ --version

9_wget http://developer.download.nvidia.com/compute/cuda/3_2_prod/toolkit/cudatoolkit_3.2.16_linux_64_ubuntu10.04.run
10_wget http://developer.download.nvidia.com/compute/cuda/3_2_prod/sdk/gpucomputingsdk_3.2.16_linux.run
11_chmod +x ./cudatoolkit_3.2.16_linux_64_ubuntu10.04.run
16_chmod +x ./gpucomputingsdk_3.2.16_linux.run

12_sudo ./cudatoolkit_3.2.16_linux_64_ubuntu10.04.run
leave default options
13_echo "export CUDAHOME=/usr/local/cuda" >> /home/.bashrc
14_echo "export PATH=$PATH:/usr/local/cuda/bin" >> /home/.bashrc
15_echo "export LD_LIBRARY_PATH=/usr/local/cuda/lib64" >> /home/.bashrc

17_sudo ./gpucomputingsdk_3.2.16_linux.run
18_echo "export NVIDIA_CUDA_SDK_LOCATION=/root/NVIDIA_GPU_Computing_SDK/C" >> /home/.bashrc  # This path may differ
19_cd /root/NVIDIA_GPU_Computing_SDK/C/common  # This path may differ
20_make

source /home/.bashrc

21_cd gem5
22_scons build/X86_VI_hammer_GPU/gem5.opt --default=X86 EXTRAS=../gem5-gpu/src:../gpgpu-sim/ PROTOCOL=VI_hammer GPGPU_SIM=True 
adding this command for solving the error in line about 570
**add this in SConstruct  =>     main.Append(CXXFLAGS=['-DPROTOBUF_INLINE_NOT_IN_HEADERS=0']) 
**also you may need to add ! instead of ~ in gem5/src/dev/copy_engine.cc  # Probably related to gcc-7
**you may have isnan error that you can fix it and use std::isnan in this path /home/gem5gpu/base/gem5/build/X86_VI_hammer_GPU/gpgpu-sim/cuda-sim/instructions.cc (there are 4 places that you should change, just follow the line#)

