# Bitbucket mhri installation

**작성자**: 장민수 (minsu@etri.re.kr)

**라이센스**: 미정

## Prerequisites

### ROS catkin tools
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -sc` main" > /etc/apt/sources.list.d/ros-latest.list'
wget http://packages.ros.org/ros.key -O - | sudo apt-key add -
sudo apt-get update
sudo apt-get install python-catkin-tools
```
Reference: http://catkin-tools.readthedocs.io/en/latest/installing.html

### Numerical Analysis Libraries
```
sudo apt-get install libblas-dev liblapack-dev libatlas-base-dev gfortran
```

### Python Package Manager
```
sudo apt-get install python-pip
pip install --upgrade pip
```

### Numpy, Scipy, Tensorflow (GPU version), Keras (1.2.0)
```
pip install numpy
pip install scipy
pip install tensorflow-gpu
pip install keras==1.2.0
```
