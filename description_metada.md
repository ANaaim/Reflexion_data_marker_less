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
| name              | type      | Example                 |
| ----------------- | --------- | ----------------------- |
| camera_ID         | str       | 'camera_01'             |
| camera_model      | str       | 'GoPro Hero 7'          |
| resolution        | list[int] | [widthxheight] in pixel |
| frame_rate        | int       | 30                      |
| frame_count       | int       | 1000                    |
| compression_codec | str       | 'H.264'                 |
| lossless          | bool      | True or False           |
| distorsion_model  | str       | 'pinhole'/'fisheye'     |
| undistort         | bool      | True or False           |

### Optional metadata (but that could be good practice to have them for example in the case of publications)
| name              | type        | Example              |
| ----------------- | ----------- | -------------------- |
| focal_lenght_mm   | float       | 4.5                  |
| aperture_f_number | float 2.8   |
| sensor_size_mm    | list[float] | [widthxheight] in mm |
| shutter_speed     | float       | 1/60                 |
| iso               | int         | 100                  |
| gain_db           | float       | 0.0                  |
| white_balance     | str         | 'auto'               |
| lens_type         | str         | 'wide'               |


## 00_calibration_data
cf metadata video.

## 01_data_video
cf metadata video.
Here the calib_matrix is not considered as a metadata, but as an individual data in a toml file. These data will be integrated into the HDF5 file in the following step as a specific metadata. 


## 02_keypoints_2D_multisubject
All this metadata are integrated into the HDF5 file.

### Calibration matrix
| name        | type                         | Example       |
| ----------- | ---------------------------- | ------------- |
| size        | list[int]                    | [ 1920, 1080] |
| matrix      | list[list[float]] 3x3 matrix |               |
| distortions | list[float]                  |               |
| rotation    | list[float]                  |               |
| translation | list[float]                  |               |

#### description of the metadata
- __size__ : Correspond to the size of the image used 
- __Matrix__ :
- __distortions__ : Correspond to the distorsion coefficients used in the camera calibration. it can be a list of 5 or 14 values depending on the model used.
- __rotation__ : 
- __translation__ :
  
### Metadata keypoint detection
One of the problem here is that we have to deal with different methods used to detect the keypoints.The methods used does not have the same parameters. As a result we should only propose a common structure for the metadata to incite people to structure their metadata and to give them. 

| name              | type | Example                |
| ----------------- | ---- | ---------------------- |
| methode_name      | str  | 'OpenPose'             |
| method_parameters | dict | ....                   |
| bbox              | bool | True                   |
| keypoints_indice  | dict | {'nose': 0, 'neck': 1} |
| frame_per_second  | int  | 30                     |

#### description of the metadata
- __methode_name__ : Name of the method used to detect the keypoints. It can be 'OpenPose', 'AlphaPose', 'PoseNet', 'RTMpose2D' or any other method used.
- __method_parameters__ : A dictionary of parameters used to obtain the 2D keypoints. These parameters are specific to the method used and will be difficult to define a common structure. Some examples:
  - threshold: float 0.5 : threshold used to filter the keypoints.
  - bbox: bool True : if the method used detect the bbox or not.
- __bbox__ : If the method used detect the bbox or not. If True, we should have a metadata for the bbox in the HDF5 file.
- __keypoints_indice__ : A dictionary of indices to name (or the opposite) to have the name of the keypoints in the array in the HDF5 file. 
- __frame_per_second__ : The frame rate of the processed point (can be different from the original video).
- 
### Metadata video
Probably all the former metada field from the 01_data_video should be integrated into the HDF5 file here under the 'video' group.
['video']
cf metadata video.

### Metadata data_set
Do we want here to know the name of the subject,session and trial ==> yes, but should only be for the subject name an ID of the subjects.(Any information about the subject should be in a separated file). Could look like this:

## 03_keypoints_2D_monosubject
Should we integrate all the metadata from the 02_keypoints_2D_multisubject. 


## 04_keypoints_3D_monosubject

We use in it the same metadata as in the 02_keypoints_2D_multisubject if from_2D_data is True.
If from_2D_data is False, we only used the metadata from the 01_data_video.

### Metadata triangulation or Method used to obtain the 3D keypoints
| name               | type   | example                     |
| ------------------ | ------ | --------------------------- |
| from_2D_data       | bool   | True                        |
| methode_name       | str    | 'weighted DDLT'/'RTMpose3d' |
| methode_parameters | dict() | {}                          |


#### description of the metadata
- __from_2D_data__ : If the 3D keypoints are obtained from triangulated 2D data. If True, we should have a metadata for the triangulation in the HDF5 file.
- __methode_name__ : Name of the method used to obtain the 3D keypoints. It can be 'weighted DDLT', 'RTMpose3d' or any other method used.
- __methode_parameters__ : A dictionary of parameters used to obtain the 3D keypoints. These parameters are specific to the method used and will be difficult to define a common structure. Some examples:
  - threshold: float 0.5 : threshold used to filter the keypoints.
  



