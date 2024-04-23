# lab
ter1
mkdir -p ~/ros2_ws/src


Inside this workspace, there is a directory called src. This folder contains all the packages created. Every time you create a package, you must be in the directory ros2_ws/src.

cd ~/ros2_ws/src
ros2 pkg create --build-type ament_python my_package --dependencies rclpy
Execute in Terminal #1

gedit ~/.bashrc

source /opt/ros/humble/setup.bash
source ~/ros2_ws/install/setup.bash
source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash
export SVGA_VGPU10=0
export ROS_DOMAIN_ID=1 

ROS_DOMAIN_ID should be different for all laptops and desktops. 


ros2 pkg list: Gives you a list with all packages in your ROS system.


cd ~/ros2_ws
colcon build



1. Create a Python file that will be executed in the my_package (all Python scripts) directory inside my_package folder.


Execute in Terminal #1

src/my_package/my_package
touch sample.py
chmod +x sample.py
Type the following code


#!/usr/bin/env python3
import rclpy
from rclpy.node import Node

class MyNode(Node):  #MIDIFY NAME OF THE CLASS
    def __init__(self):
        # call super() in the constructor in order to initialize the Node object
        # the parameter we pass is the node name
        super().__init__('sample') #MIDIFY NAME OF THE NODE
        # create a timer sending two parameters:
        # - the duration between 2 callbacks (0.2 seeconds)
        # - the timer function (timer_callback)
        self.create_timer(0.2, self.timer_callback)
        
    def timer_callback(self):
        # print a ROS2 log on the terminal with a great message!
        self.get_logger().info("Congratulations!")
        
        
def main(args=None):
    # initialize the ROS communication
    rclpy.init(args=args)
    # declare the node constructor
    node = MyNode() #MODIFY NAME OF THE NODE
    # pause the program execution, waits for a request to kill the node (ctrl+c)
    rclpy.spin(node)
    # shutdown the ROS communication
    rclpy.shutdown()
    
    
if __name__ == '__main__':
    main()

Modify the setup.py file to generate an executable from the Python file you just created.

from setuptools import setup

package_name = 'my_package'

setup(
    name=package_name,
    version='0.0.0',
    packages=[package_name],
    data_files=[
        ('share/ament_index/resource_index/packages',
            ['resource/' + package_name]),
        ('share/' + package_name, ['package.xml']),
    ],
    install_requires=['setuptools'],
    zip_safe=True,
    maintainer='asha',
    maintainer_email='asha@todo.todo',
    description='TODO: Package description',
    license='TODO: License declaration',
    tests_require=['pytest'],
    entry_points={
        'console_scripts': [
        'sample = my_package.sample:main'
        ],
    },
)


Execute in Terminal #1

cd ~/ros2_ws
colcon build
source ~/ros2_ws/install/setup.bash
ros2 run  my_package sample

Creating Launch Files

Execute in Terminal #2

cd ~/ros2_ws/src/my_package
mkdir launch
cd launch
touch my_package_launch_file.launch.py
chmod +x my_package_launch_file.launch.py
