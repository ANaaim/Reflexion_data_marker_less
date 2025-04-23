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



## 00_calibration_data

## 01_data_video
Metadata should be at the leaf level of the 01_data_video. 


## 02_keypoints_2D_multisubject
Metadata should be integrated into the HDF5 file.
List useful metadata fields for the HDF5 file:
- 'methode_name'-> str: 'OpenPose' non optional 
- 'methode_version'-> str: '1.7.0' optional
-  Is bbox in the HDF5 file? -> bool: True or False.
- dictionary indices to name (or the opposite) to have the name of the keypoints in the array in the HDF5 file. 

Probably all the former metada field from the 01_data_video should be integrated into the HDF5 file here under the 'video' group.
['video']


## 03_keypoints_2D_monosubject



## 04_keypoints_3D_monosubject

