# Installation Guide

The [`JetPack SDK`](https://developer.nvidia.com/embedded/jetpack-sdk-62) upgradation with isaac ros implementation for Nvidia Jeston series.


## Set up
Frist Check your JetPack version and makesure it upgrade to the version you want to use.
```shell
$ cat /etc/nv_tegra_release
```
Terminal should show like below
```shell
# R36 (release), REVISION: 4.3, GCID: 38968081, BOARD: generic, EABI: aarch64, DATE: Wed Jan  8 01:49:37 UTC 2025
# KERNEL_VARIANT: oot
TARGET_USERSPACE_LIB_DIR=nvidia
TARGET_USERSPACE_LIB_DIR_PATH=usr/lib/aarch64-linux-gnu/nvidia
```
For example, using `JetPack 6.2` means `L4T 36.4.3`, which shows `# R36 (release), REVISION: 4.3`,can check the version at [`JetPack Archive`](https://developer.nvidia.com/embedded/jetpack-archive)

Upgrade to newest JetPack
```shell
$ sudo apt update
$ apt list --upgradable
$ sudo apt upgrade
$ reboot
```

Or Upgrade to specific JetPack version
```shell
$ sudo vi /etc/apt/source.list.d/nvidia-l4t-apt-source.list
# modify source list, add the vesion you want
deb https://repo.download.nvidia.com/jeston/common <release vesion> main
deb https://repo.download.nvidia.com/jeston/<platform> <release vesion> main
```
P.S. #<platform> should be t186, t194...etc. please check [`Tegra wiki`](https://en.wikipedia.org/wiki/Tegra)

#Build isaac ros apriltag

Install LFS
```shell
$ sudo apt install git-lfs
$ git lfs install --skip-repo
```
Clone package isaac ros apriltage and depend packages, here using release-3.2.
```shell
$ mkdir -p <your_workspace>/src
$ cd <your_workspace>/src
$ git clone -b release-3.2 https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common.git
$ git clone -b release-3.2 https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_apriltag.git
$ git clone -b release-3.2 https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros
$ git clone -b release-3.2 https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_examples
$ cd ..
```
Install depends through rosdep
```shell
$ mkdir -p <your_workspace>/src
$ rosdep install --from-paths src --ignore-src -r -y
```
Get [`negotiated`](https://github.com/osrf/negotiated)
```shell
$ cd <your_workspace>/src
$ git clone https://github.com/osrf/negotiated
$ cd ..
```
Modify CMakeList.txt in `isaac_ros_nitros`
Add `find_package(negotiated)`
Build packages

Install [`nvcv`] https://github.com/CVCUDA/CV-CUDA/releases
```shell
$ wget https://github.com/CVCUDA/CV-CUDA/releases/download/v0.5.0-beta/nvcv-lib-0.5.0_beta_DP-cuda12-aarch64-linux.deb
$ wget https://github.com/CVCUDA/CV-CUDA/releases/download/v0.5.0-beta/nvcv-dev-0.5.0_beta_DP-cuda12-aarch64-linux.deb
$ sudo dpkg -i nvcv-lib-0.5.0_beta_DP-cuda12-aarch64-linux.deb
$ sudo dpkg -i nvcv-dev-0.5.0_beta_DP-cuda12-aarch64-linux.deb
```

```shell
$ colcon build --symlink-install
```

#Test with example
Follow the instruction in `Run Launch File` to test.[Isaac ROS AprilTag](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html#quickstart). 


