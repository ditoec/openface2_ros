# ROS Wrapper for Openface 2.1.0

This is a ROS wrapper for OpenFace 2.1.0. In particular, given an image of a face, it will output:
  * Eye Gaze Vectors
  * Head Pose
  * 2D Landmarks
  * 3D Landmarks
  * Action Units
  * Debug Visualization (optional)

## ROS Parameters

### Required
  * `~image_topic` - The topic the image is provided on (e.g. `/usb_cam/image_raw`).

### Optional
  * `~publish_viz` - Set to `true` to publish a debug visualization (default: `false`).

## Installation

* Clone the OpenFace project release 2.1.0 : `git clone https://github.com/TadasBaltrusaitis/OpenFace/tree/OpenFace_2.1.0`
* Download model : [Model download · TadasBaltrusaitis/OpenFace Wiki · GitHub](https://github.com/TadasBaltrusaitis/OpenFace/wiki/Model-download)
* Install OpenFace, following this instruction : [Unix Installation · TadasBaltrusaitis/OpenFace Wiki · GitHub](https://github.com/TadasBaltrusaitis/OpenFace/wiki/Unix-Installation/8c2096c032066021ebe680b00c22964c6d473027) (don't forget to sudo make install at the end)
* Download [openface2_ros](https://github.com/ditoec/openface_ros) to your catkin src folder and `cd .. && catkin make`
* Clone [`usb_cam` ros node](http://wiki.ros.org/usb_cam) or other ros node of your choice for interfacing with USB camera

### Running

* `roscore`
* `rosrun usb_cam usb_cam_node`
* `roslaunch openface2_ros openface2_ros.launch`

### Notes

This node requires `cv_bridge` *and* OpenCV 3. You must ensure that `cv_bridge` is also linked against OpenCV 3. If you get a warning during compilation, you may have to manually clone the `vision_opencv` repository and re-build `cv_bridge`.

### Issues

If running `openface2_ros` results in a segfault or you see the following lines when running `catkin_make`:

    /usr/bin/ld: warning: libopencv_imgproc.so.2.4, needed by /opt/ros/indigo/lib/libcv_bridge.so, may conflict with libopencv_imgproc.so.3.1
    /usr/bin/ld: warning: libopencv_highgui.so.2.4, needed by /opt/ros/indigo/lib/libcv_bridge.so, may conflict with libopencv_highgui.so.3.1
    /usr/bin/ld: warning: libopencv_calib3d.so.2.4, needed by /usr/lib/x86_64-linux-gnu/libopencv_contrib.so.2.4.8, may conflict with libopencv_calib3d.so.3.1

then openface2 ros is linking against OpenCV2 instead of OpenCV3. To fix this: update cmake to at least 3.6.2, rebuild OpenCV3, clone vision\_opencv into the src folder of your catkin workspace, then recompile cv\_bridge. Remake your catkin workspace, and the segfault and warnings should have been resolved.

## Published Topics

* `/openface2/faces` ( `openface2_ros/faces` )
* `/openface2/image` ( `sensor_msgs/Image` )

## Messages

### Faces
```
std_msgs/Header header

openface_ros/Face[] faces
uint32 count```
```

### FaceFeatures
```
geometry_msgs/Vector3 left_gaze
geometry_msgs/Vector3 right_gaze
geometry_msgs/Vector3 gaze_angle

geometry_msgs/Pose head_pose

geometry_msgs/Point[] landmarks_3d
geometry_msgs/Point[] landmarks_2d

openface_ros/ActionUnit[] action_units
```

### ActionUnit
```
string name
float64 presence
float64 intensity
```

## Changelog
* Accommodate major changes in openface 2.0.6 (functions and classes naming & grouping)
* Add eye gaze angle in the Face message type
* Add Faces message type (an array of Face message type) in order to accommodate the new multiple face detection in openface 2.0
