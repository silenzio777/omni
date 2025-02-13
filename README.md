lsusb
sudo apt install nano
sudo add-apt-repository universe

mkdir usbhdd

# find name of usb hdd disk partition (/dev/sdb4 for example)
sudo nano /etc/fstab

# add line at the end
/dev/sdb4 usbhdd ext4 defaults 0 0

ls -l
...
drwxr-xr-x 4 root   root   4096 Feb 13 09:59 usbhdd

sudo chown ubuntu:ubuntu usbhdd

ls -l
drwxr-xr-x 4 ubuntu ubuntu 4096 Feb 13 09:59 usbhdd

ls usbhdd/
install-logs-2025-02-13.0  lost+found






------------------------------------------------------------------------------------------
http://www.yahboom.net/study/Orin-NX-SUPER
------------------------------------------------------------------------------------------

https://developer.nvidia.com/embedded/jetson-linux-r3643




It should be something like this:


tar xf Jetson_Linux_R36.4.3_aarch64.tbz2 

sudo tar xpf Tegra_Linux_Sample-Root-Filesystem_R36.4.3_aarch64.tbz2 -C Linux_for_Tegra/rootfs/ 

cd Linux_for_Tegra/

sudo ./tools/l4t_flash_prerequisites.sh 

sudo ./apply_binaries.sh


------------------------------------------------------------------------------------------
## MOUNT NEW USB HDD 
------------------------------------------------------------------------------------------
# https://askubuntu.com/questions/125257/how-do-i-add-an-additional-hard-drive

sudo fdisk -l

Disk /dev/sda: 232.91 GiB, 250059350016 bytes, 488397168 sectors
Disk model: TM3250820AS     
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: AC243013-F3C2-4244-9B66-D10D60D626B2

Device     Start       End   Sectors   Size Type
/dev/sda1   2048 488396799 488394752 232.9G Linux filesystem


2.1 Create a mount point
sudo mkdir /hdd  <<<<<<<------ NAME OF YOUR DEV "backup" (for example)

2.2 Edit /etc/fstab
Open /etc/fstab file with root permissions:

sudo nano /etc/fstab
And add following to the end of the file:

/dev/sda1    /hdd    ext4    defaults    0    0

2.3 Mount partition
Last step and you're done!

sudo mount /hdd

## WORK

------------------------------------------------------------------------------------------
## CHANGE DIR AND FILE PERMISSIONS
------------------------------------------------------------------------------------------
# https://askubuntu.com/questions/6723/change-folder-permissions-and-ownership

First, check demo.txt permissions:

# ls -l demo.txt
Out:

-rw-r--r-- 1 root root 0 Aug 31 05:48 demo.txt
In this example change file ownership to vivek user and list the permissions:

# chown vivek demo.txt
# ls -l demo.txt
Out:

-rw-r--r-- 1 vivek root 0 Aug 31 05:48 demo.txt
In this next example, the owner is set to vivek followed by a colon and group ownership is also set to vivek group, run:

# chown vivek:vivek demo.txt <<<<<<<<<<-----------------
# ls -l demo.txt
Out:

-rw-r--r-- 1 vivek vivek 0 Aug 31 05:48 demo.txt



------------------------------------------------------------------------------------------
## RUN sudo apt install on CLEAR UBUNTU SYSTEM 
------------------------------------------------------------------------------------------

## RUN:
sudo add-apt-repository universe

## AFTER INSTALL rep atom apt...
sudo apt install mc




------------------------------------------------------------------------------------------
## VINO INSTALL
------------------------------------------------------------------------------------------
# https://redos.red-soft.ru/base/redos-7_3/7_3-remote-access/7_3-vino/?nocache=1739389433387
sudo apt update
sudo apt install vino

# Set vino's password through terminal
gsettings set org.gnome.Vino vnc-password $(echo -n 'mypasswd'|base64)

gsettings set org.gnome.Vino authentication-methods "['vnc']"
gsettings set org.gnome.Vino require-encryption false
gsettings set org.gnome.Vino prompt-enabled false

## WORK !!
## RUN:
/usr/lib/vino/vino-server

-------------------------------------------------------------------------
## Install OpenCV on Jetson Nano
-------------------------------------------------------------------------
## https://qengineering.eu/install-opencv-on-jetson-nano.html



$ cd ~
$ wget -O opencv.zip https://github.com/opencv/opencv/archive/4.8.0.zip
$ wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.8.0.zip
# unpack
$ unzip opencv.zip
$ unzip opencv_contrib.zip
# some administration to make live easier later on
$ mv opencv-4.8.0 opencv
$ mv opencv_contrib-4.8.0 opencv_contrib
# clean up the zip files
$ rm opencv.zip
$ rm opencv_contrib.zip


$ cd ~/opencv
$ mkdir build
$ cd build

$ cmake -D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr \
-D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
-D EIGEN_INCLUDE_PATH=/usr/include/eigen3 \
-D WITH_OPENCL=OFF \
-D WITH_CUDA=ON \
-D CUDA_ARCH_BIN=5.3 \
-D CUDA_ARCH_PTX="" \
-D WITH_CUDNN=ON \
-D WITH_CUBLAS=ON \
-D ENABLE_FAST_MATH=ON \
-D CUDA_FAST_MATH=ON \
-D OPENCV_DNN_CUDA=ON \
-D ENABLE_NEON=ON \
-D WITH_QT=OFF \
-D WITH_OPENMP=ON \
-D BUILD_TIFF=ON \
-D WITH_FFMPEG=ON \
-D WITH_GSTREAMER=ON \
-D WITH_TBB=ON \
-D BUILD_TBB=ON \
-D BUILD_TESTS=OFF \
-D WITH_EIGEN=ON \
-D WITH_V4L=ON \
-D WITH_LIBV4L=ON \
-D WITH_PROTOBUF=ON \
-D OPENCV_ENABLE_NONFREE=ON \
-D INSTALL_C_EXAMPLES=OFF \
-D INSTALL_PYTHON_EXAMPLES=OFF \
-D PYTHON3_PACKAGES_PATH=/usr/lib/python3/dist-packages \
-D OPENCV_GENERATE_PKGCONFIG=ON \
-D BUILD_EXAMPLES=OFF ..

make -j4


sudo rm -r /usr/include/opencv4/opencv2
sudo make install
sudo ldconfig
# cleaning (frees 300 MB)

make clean
sudo apt-get update

# Checking.
Now it is time to check your installation. It can be done in a fast way by using Python. Use the commands shown in the screen dump below. It all speaks for itself.


python3
import cv2
cv2.__version__



OpenCV will be installed to the /usr directory, all files will be copied to following locations:

    /usr/bin - executable files
    /usr/lib/aarch64-linux-gnu - libraries (.so)
    /usr/lib/aarch64-linux-gnu/cmake/opencv4 - cmake package
    /usr/include/opencv4 - headers
    /usr/share/opencv4 - other files (e.g. trained cascades in XML format)



-------------------------------------------------------------------------
## How To Install CMake 3.22 Ubuntu 22.04
-------------------------------------------------------------------------
## https://gist.github.com/UbuntuEvangelist/afd13e6fba7ffc5dbf7c5da31b55dff6#file-how-to-install-cmake-3-22-ubuntu-22-04-L3


sudo apt update
sudo apt install build-essential 
sudo apt install libssl-dev
export OPENSSL_ROOT_DIR=/usr/include/openssl
wget https://cmake.org/files/v3.29/cmake-3.29.2.tar.gz
tar -xzvf cmake-3.29.2.tar.gz
cd cmake-3.29.2
./bootstrap
make -j$(nproc)
sudo make install
# Update PATH Environment Variable
which cmake
/usr/local/bin/cmake
export PATH=/usr/local/bin/cmake:$PATH
source ~/.bashrc
cmake --version



gsettings set org.gnome.desktop.background picture-uri ""
gsettings set org.gnome.desktop.background picture-uri-dark ""
gsettings set org.gnome.desktop.background primary-color '#101010' 
