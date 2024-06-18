# ROS-GZ Modified for MAC OS (Apple Silicon)

- ROS2 Jazzy - Gazebo Harmonic

## Prerequisites

- Install ROS2 Jazzy and Gazebo Harmonic with (https://github.com/IOES-Lab/ROS2_Jazzy_MacOS_Native_AppleSilicon)

  ```bash
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/IOES-Lab/ROS2_Jazzy_MacOS_Native_AppleSilicon/main/install.sh)"
  ```

- Download source codes
  
  ```bash
  mkdir -p ~/ros_gz_ws/src && cd ~/ros_gz_ws/src
  git clone https://github.com/IOES-Lab/ros_gz_for_mac
  git clone https://github.com/rudislabs/actuator_msgs.git
  git clone -b 2.0.4 https://github.com/swri-robotics/gps_umd.git 
  git clone -b release/jazzy/vision_msgs/4.1.1-3 https://github.com/ros2-gbp/vision_msgs-release.git vison_msgs

  # Remove failing code
  rm -rf gps_umd/gpsd_client gps_umd/gps_umd gps_umd/gps_tools
  # Modify code to avoid compile fail
  nano $HOME/gz_harmonic/install/lib/cmake/gz-msgs10/gz-msgs10-targets.cmake
  # Find TINYXML2::TINYXML2 and remove it (keep original commented out!)
  # (search text with Ctrl+W)
  ```

- Compile
  
  ```bash
  cd ~/ros_gz_ws
  export GZ_CONFIG_PATH=$HOME/gz_harmonic/install/bin/gz
  export GZ_VERSION=harmonic
  colcon build --merge-install --cmake-args \
      -DCMAKE_BUILD_TYPE=RelWithDebInfo \
      -DCMAKE_MACOSX_RPATH=FALSE \
      -DCMAKE_CXX_FLAGS='-std=c++17' \
      -DCMAKE_OSX_ARCHITECTURES=arm64 \
      -DBUILD_TESTING=OFF \
      -DCMAKE_BUILD_TYPE=Release
  ```

- Post 
  ```
  # Put it back
  nano $HOME/gz_harmonic/install/lib/cmake/gz-msgs10/gz-msgs10-targets.cmake
  # Put back TINYXML2::TINYXML2

  # Source
  source install/setup.zsh
  # add to $HOME/ros2_jazzy/activate_ros
  ```