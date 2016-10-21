# OpenFace for ROS

This is a small ROS node that exposes OpenFace over ROS. In particular, given a image of a face, it will output:
  * Eye Gaze Vectors
  * Head Pose
  * 2D Landmarks
  * 3D Landmarks
  * Action Units
  * Debug Visualization (optional)

This repository expects interaction lab's fork of OpenFace to be installed to /usr/local and OpenCV 3.

## ROS Parameters

### Required
  * `~image_topic` - The topic the image is provided on (e.g. `/usb_cam/image_raw`).

### Optional
  * `~clnf_model_path` - Provide an alternate CLNF model to OpenFace.
  * `~tri_model_path` - Provide an alternate tri model to OpenFace.
  * `~au_model_path` - Provide an alternate AU model to OpenFace.
  * `~publish_viz` - Set to `true` to publish a debug visualization (default: `false`).

## Notes

This node requires cv_bridge *and* OpenCV 3. You must ensure that cv_bridge is also linked against OpenCV 3. If you get a warning during compilation, you may have to manually clone the `vision_opencv` repository and re-build cv_bridge.
