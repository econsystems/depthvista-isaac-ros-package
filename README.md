# DepthVistaMIPI_IRD streaming in ISAAC ROS Platform

## Prerequisites
- Jetpack version 5.1.1
- Jetson Orin / Jetson Xavier (other devkits are not supported)

## BSP Package Installation

1. Download the BSP package from [here](https://ftp.e-consystems.com/nextcloud/index.php/s/ncdNirE5DS5dp5b)

2. Copy the release package to the home directory of the Jetson development kit and run the following commands to extract the release package to obtain the binaries:

        tar -xampf e-CAM0M30_TOF_ISAAC_<L4T_Version>_<release_date>_<release_version>.tar.gz
        cd e-CAM0M30_TOF_ISAAC_<L4T_Version>_<release_date>_<release_version>

3. Run the following commands to install the binaries:

        sudo chmod a+x ./install_binaries_<version>.sh
        sudo ./install_binaries_<version>.sh

4. The above script will reboot the Jetson development kit automatically after installing the binaries successfully.

## Enumerating DepthVistaMIPI_IRD as an ISAAC ROS Argus Camera 

1. Set up your development environment by following the steps given [here](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common/blob/main/docs/dev-env-setup.md)

2. Clone the ISAAC ROS dependent repositories under `~/workspaces/isaac_ros-dev/src`

Note : (Keep repository as `/ssd/workspaces/isaac_ros-dev/src` if you have setup your docker environment in a SSD)

        cd ~/workspaces/isaac_ros-dev/src

        git clone https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common
        
        git clone https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros
        
        git clone https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_image_pipeline
        
        git clone https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_argus_camera

3. Launch the docker container using the `run_dev.sh` script in terminal 1:
        
        cd ~/workspaces/isaac_ros-dev/src/isaac_ros_common && \
        ./scripts/run_dev.sh

4. Inside the container, build and source the workspace:

        cd /workspaces/isaac_ros-dev && \
        colcon build --symlink-install && \
        source install/setup.bash

5. To stream in ISAAC ROS platform, we have two nodes: the publisher node (terminal 1) which launches the argus camera stream and the subscriber node (terminal 2) which receives the data from the publisher.

6. In terminal 1, run the following launch file to stream the argus camera:

        ros2 launch isaac_ros_argus_camera isaac_ros_argus_camera_mono.launch.py

7. Open another terminal and launch the docker container:

        cd ~/workspaces/isaac_ros-dev/src/isaac_ros_common && \
        ./scripts/run_dev.sh

8. Source the workspace in terminal 2:
        source install/setup.bash

9. In terminal 2, run the following commands to view the live stream of argus camera:

        ros2 run image_view image_view --ros-args -r image:=/left/image_raw

