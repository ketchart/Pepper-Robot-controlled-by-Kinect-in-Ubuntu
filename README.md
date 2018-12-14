# Pepper-Robot-controlled-by-Kinect-in-Ubuntu
// reference https://www.reddit.com/r/ROS/comments/6qejy0/openni_kinect_installation_on_kinetic_indigo/?fbclid=IwAR0Mj1RDO11DIJkw4KNb7wYoZ8X13c7HmV9LhSNYXNANyxDRDKXviXtrl8k

// install

// 0. Pre-requsites

sudo apt-get install ros-kinetic-openni-camera ros-kinetic-openni-launch

sudo apt-get install git build-essential python libusb-1.0-0-dev freeglut3-dev openjdk-8-jdk

sudo apt-get install doxygen graphviz mono-complete

cd ~/
mkdir kinect

// 1. Install OpenNI

cd ~/kinect
git clone https://github.com/OpenNI/OpenNI.git
cd OpenNI
git checkout Unstable-1.5.4.0
cd Platform/Linux/CreateRedist
chmod +x RedistMaker
./RedistMaker

cd /home/chong-lab/kinect/OpenNI/Platform/Linux/Redist/OpenNI-Bin-Dev-Linux-x64-v1.5.4.0
sudo ./install.sh

cd ~/kinect
git clone https://github.com/avin2/SensorKinect
cd SensorKinect
cd Platform/Linux/CreateRedist
chmod +x RedistMaker
./RedistMaker

cd /home/chong-lab/kinect/SensorKinect/Platform/Linux/Redist/Sensor-Bin-Linux-x64-v5.1.2.1
chmod +x install.sh
sudo ./install.sh

// 2. Install Kinetic OpenNI

sudo apt-get install ros-kinetic-openni*

// 3. Install NITE

cd ~/kinect
git clone https://github.com/arnaud-ramey/NITE-Bin-Dev-Linux-v1.5.2.23
cd /home/chong-lab/kinect/NITE-Bin-Dev-Linux-v1.5.2.23/x64
sudo ./install.sh

// 4. Install openni_tracker package

cd ~/catkin_ws/src
git clone https://github.com/ros-drivers/openni_tracker.git

// 5. Remake Catkin Workspace

cd ~/catkin_ws
catkin_make
catkin_make install

// launch

roscore

roslaunch openni_launch openni.launch

rosrun image_view image_view image:=/camera/rgb/image_color

rosrun image_view disparity_view image:=/camera/depth_registered/disparity

cd ~/catkin_ws/devel
source setup.bash 
or 
source ~/catkin_ws/devel/setup.bash
rosrun openni_tracker openni_tracker

// create package

roscreate-pkg kinect_pj rospy tf

cd ~/home/chong-lab/catkin_ws/src/kinect_pj/src
chmod +x kinect_pj

// launch kinect_pj_v3

cd ~/catkin_ws/devel
source setup.bash 
or 
source ~/catkin_ws/devel/setup.bash

rosrun kinect_pj_v3 kinect_pj_v3

********************************************************************************************************************

This project purpose is to initialize Kinect to mimic Human movements for Pepper Robot which can be used further in Machine Learning. 
このプロジェクトの目的は、機械学習でさらに使用できるPepper Robotのための人間の動きを模倣するためにKinectを初期化することです。

Pepper Robot is not industrial robot that we can pull his arm to teach him some movements. Therefore, he has to learn by watching our movements.

Kinect is a tool of choice for this because it can read human joints which its data will transfer to Peppr Robot.

In this project, Kinect only reads data from User Right Shoulder and Right Elbow.

In the code, the mathematics has 2 parts, Right Shoulder Rotation Matrix and Right Elbow Rotation Matrix.

Right Shoulder Rotation Matrix divided into Shoulder Pitch and Shoulder Raw. Shoulder pitch is calculated from Arctan of Position of Shoulder and Elbow in Z and X coordinate. Shouler Raw is calculated from Arctan of Position of Shoulder and Elbow in Y and X coordinate.

Right Elbow Rotation Matrix is calculated from Rotation Matrix from Shoulder's Absolute Rotation Matrix to Elbow's Absolute Rotation Matrix. This Absolute Rotation Matrix means the rotation of the joint from Kinect. Kinect provides data in Quaternion. So, the code has to transfer it to Cartesian coordinates. Finally, this Rotation matrix contains Raw, Pitch, Yaw angle that the code transfer them to Pepper Robot.

Rviz can provide the images of X, Y, Z coordinate of Human joints.

Hope this project is easy to undertand and shorten your time to start your project.
If you have any question, please feel free to ask at
ketchart.kaewplee@gmail.com
