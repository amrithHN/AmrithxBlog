---
author: amrithmmh
category:
  - uncategorized
date: "2022-06-24T08:12:24+00:00"
guid: https://armphibian.wordpress.com/?p=124
title: ROS2 HUMBLE HAWKSBILL NOTES
url: /2022/06/24/ros2-humble-hawksbill-notes/

---
![](/wp-content/uploads/2022/06/humble-small.png?w=150)

Installation : follow the instruction: https://docs.ros.org/en/humble/Installation/Alternatives/Ubuntu-Development-Setup.html

**Creating a workspace:**

```
mkdir -p ros2_ws/src && cd ros2_ws
colcon build
source install/local_setup.bash && source install/setup.bash
```

**Creating a package:**(Python based)

ament\_python should have the following dir structure

```
.
└── my_project
    ├── CMakeLists.txt
    ├── package.xml
    └── my_project
        ├── __init__.py
        └── my_script.py
```

```
cd ros2_ws/src
ros2 pkg create <pkg-name> --dependencies [deps] --build-type ament_python
cd ros2_demo_py/
rm CMakeLists.txt
touch setup.py setup.cfg

```

setup.cfg should have:

```
[develop]
script_dir=$base/lib/<package-name>
[install]
install_scripts=$base/lib/<package-name>
```

setup.py should have:

```
import os
from glob import glob
from setuptools import setup

package_name = 'my_package'

setup(
    name=package_name,
    version='0.0.0',
    # Packages to export
    packages=[package_name],
    # Files we want to install, specifically launch files
    data_files=[
        # Install marker file in the package index
        ('share/ament_index/resource_index/packages', ['resource/' + package_name]),
        # Include our package.xml file
        (os.path.join('share', package_name), ['package.xml']),
        # Include all launch files.
        (os.path.join('share', package_name, 'launch'), glob(os.path.join('launch', '*.launch.py'))),
    ],
    # This is important as well
    install_requires=['setuptools'],
    zip_safe=True,
    author='ROS 2 Developer',
    author_email='ros2@ros.com',
    maintainer='ROS 2 Developer',
    maintainer_email='ros2@ros.com',
    keywords=['foo', 'bar'],
    classifiers=[
        'Intended Audience :: Developers',
        'License :: TODO',
        'Programming Language :: Python',
        'Topic :: Software Development',
    ],
    description='My awesome package.',
    license='TODO',
    # Like the CMakeLists add_executable macro, you can add your python
    # scripts here.
    entry_points={
        'console_scripts': [
            'my_script = my_package.my_script:main'
        ],
    },
)
```

\_\_init\_\_.py can be empty follow this [link](https://docs.ros.org/en/humble/How-To-Guides/Ament-CMake-Python-Documentation.html) to learn more

package.xml should have :

```
<buildtool_depend>ament_cmake_python</buildtool_depend>
```

CMakeLists.txt should have:

```
find_package(ament_cmake_python REQUIRED)
# ...
ament_python_install_package(${PROJECT_NAME})
```

build package:

```
cd ros2_ws
colcon build --symlink-install
source install/setup.bash
```

run the package:

```
ros2 run <package name> <name of script in setup.py>
```

creating launch file:

```
mkdir launch
touch launch/launch.py
```

launch.py

```
from launch import LaunchDescription
from launch_ros.actions import Node

def generate_launch_description():
    return LaunchDescription([
        Node(
            package='turtlesim',
            namespace='turtlesim1',
            executable='turtlesim_node',
            name='sim'
        ),
        Node(
            package='turtlesim',
            namespace='turtlesim2',
            executable='turtlesim_node',
            name='sim'
        ),
        Node(
            package='turtlesim',
            executable='mimic',
            name='mimic',
            remappings=[
                ('/input/pose', '/turtlesim1/turtle1/pose'),
                ('/output/cmd_vel', '/turtlesim2/turtle1/cmd_vel'),
            ]
        )
    ])
```

add dependency to package.xml

```
<exec_depend>ros2launch</exec_depend>
```

```
cd launch
ros2 launch launch.py
```
