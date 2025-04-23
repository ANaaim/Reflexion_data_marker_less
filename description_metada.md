## Data set metadata 
depth : correspond to the number of nested folders before going to the data folder. Each depth will correspond to a type of analysis, could be a subject, session or trial in any order. 

then the complete data set will be describe using a series of nested dictionaries. Example: 
```python
depth = 3
{'subject_01': {'session_01': ['trial_01', 'trial_02', 'trial_03'],
                 'session_02': ['trial_01', 'trial_02', 'trial_03']},
 'subject_02': {'session_01': ['trial_01', 'trial_02', 'trial_03'],
                'session_02': ['trial_01', 'trial_02', 'trial_03']}}
```
From there it will be easy to access all the data just using the dictionary keys, and to process automatically all the data. 

## Medata video 
These metadata are common to all the video files (00_calibration_data, 01_data_video)

### Non optional metadata
camera_ID: str 'camera_01'
camera_model: str 'GoPro Hero 7'
resolution:list[int] [widthxheight] in pixel
frame_rate: int 30
frame_count: int 1000
compression_codec: str 'H.264'
lossless: bool: True or False
distorsion_model: str: 'pinhole',"fisheye
undistort: bool: True or False

### Optional metadata (but that could be good practice to have them for example in the case of publications)
focal_lenght_mm: float 4.5
aperture_f_number: float 2.8
sensor_size_mm: list[float] [widthxheight] in mm
shutter_speed: float 1/60
iso: int 100
gain_db: float 0.0
white_balance: str 'auto'
lens_type: str 'wide'


## 00_calibration_data
cf metadata video.

## 01_data_video
cf metadata video.
Here the calib_matrix is not considered as a metadata, but as an individual data in a toml file. These data will be integrated into the HDF5 file in the following step as a specific metadata. 


## 02_keypoints_2D_multisubject
All this metadata are integrated into the HDF5 file.

List useful metadata fields for the HDF5 file:
'methode_name'-> str: 'OpenPose' non optional 
'methode_version'-> str: '1.7.0' optional
Is bbox in the HDF5 file? -> bool: True or False.
dictionary indices to name (or the opposite) to have the name of the keypoints in the array in the HDF5 file. 
calibration

Probably all the former metada field from the 01_data_video should be integrated into the HDF5 file here under the 'video' group.
['video']
cf metadata video.


## 03_keypoints_2D_monosubject



## 04_keypoints_3D_monosubject

