# microros_android_example
This repository provides an example on how to use microros on an Android device.
The provided code is intended to be used with ROS2 Humble. However, it is possible to use other microros-supported distributions and adapt the installation steps. 

Setup instructions

1) Install `ROS2` on your device. In this example, I installed `ROS2 Humble` on a laptop running `Ubuntu 22.04 through WSL2` (https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html).
2) Set up the ROS2 environment by running: ```source /opt/ros/humble/setup.bash```.
   To avoid doing this every time, modify your `~/.bashrc file` by inserting the line `source /opt/ros/humble/setup.bash`.
   Then, either close the terminal and open it again, or run ```source ~/.bashrc```.
3) Create a ROS2 workspace
   `mkdir <ROS2_WS_NAME> && cd <ROS2_WS_NAME>`
   E.g., ```mkdir microros_ws && cd microros_ws```
5) Clone microros repository
   ```git clone -b humble https://github.com/micro-ROS/micro_ros_setup.git src/micro_ros_setup```
6) Install the dependencies required by microros to build the firmware
   ```sudo apt install python3-rosdep```
7) Install Android Studio and both Android SDK and NDK for WSL2
   Instructions on how to properly install and run Android Studio on WSL2 can be found online. For instance, you can follow this video: https://www.youtube.com/watch?v=XJ0dI2SYHIE.
   Then, you can install the latest Android NDK from Android Studio using the SDK manager. You can follow these steps: https://developer.android.com/studio/projects/install-ndk#specific-version
8) Define the environment variables related to the Android environment. You can modify your `~/.bashrc file` by inserting the following lines:
   `# Android environment variables
   export ANDROID_HOME='<ANDROID_SDK_DIRECTORY>'
   export ANDROID_NDK='<ANDROID_NDK_DIRECTORY>'
   export ANDROID_NATIVE_API_LEVEL=<ANDROID_API_LEVEL>
   export ANDROID_ABI=<ANDROID_ABI>
   `
   The `ANDROID_HOME` variable is needed to specify the location of Android SDK, while the `ANDROID_NDK` variable specifies the location of the installed Android NDK.
   The other two variables are needed to specify the Android API level used for the application, and the Application Binary Interface (ABI) that is needed to define the CPU instructions set and other parameters that depend on the Android device that is being used (see https://developer.android.com/ndk/guides/abis for more details).

   For this example, the following environment variable were set:
   ```
   # Android environment variables
   export ANDROID_HOME='~/android_sdk/'
   export ANDROID_NDK='~/android_sdk/ndk/25.2.9519653'
   export ANDROID_NATIVE_API_LEVEL=android-31
   export ANDROID_ABI=arm64-v8a
   ```
9) Update the ROS2 dependency information and install the system dependencies required by the ROS2 packages in your workspace.
   ```
   rosdep update && rosdep install --from-paths src --ignore-src -y
   ```
   If you encounter an error such as: `ERROR: no sources directory exists on the system meaning rosdep has not yet been initialized.`, run the following command before trying again:
   ```sudo rosdep init```

10) Build the packages in you workspace.
    ```
    cd <ROS2_WS_NAME>
    colcon build
    source install/local_setup.bash
    ```
11) Create the firmware for the Android platform
    ```ros2 run micro_ros_setup create_firmware_ws.sh android generic```
    If an error such as `touch: cannot touch 'mcu_ws/ros2/rcl_logging/rcl_logging_log4cxx/COLCON_IGNORE': No such file or directory` is raised, proceed as follows:
    * Open the file `<ROS_WS_NAME>/src/micro_ros_setup/config/android/generic/create.sh
    * Comment the line `touch mcu_ws/ros2/rcl_logging/rcl_logging_log4cxx/COLCON_IGNORE`
    * Repeat from step #10
12) Verify that no configuration step is needed for the firmware
    ```ros2 run micro_ros_setup configure_firmware.sh```
    This should output: `No configuration step needed for generic platform generic`.
13) TODO Finish


