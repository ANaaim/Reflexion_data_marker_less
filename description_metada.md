## 00_calibration_data

## 01_data_video
Metadata should be at the leaf level of the 01_data_video. 


## 02_keypoints_2D_multisubject
Metadata should be integrated into the HDF5 file.
List useful metadata fields for the HDF5 file:
- 'methode_name'-> str: 'OpenPose' non optional 
- 'methode_version'-> str: '1.7.0' optional

Probably all the former metada field from the 01_data_video should be integrated into the HDF5 file here under the 'video' group.
['video']


## 03_keypoints_2D_monosubject



## 04_keypoints_3D_monosubject

