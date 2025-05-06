# Metadata description

## Metadata data set 
depth : correspond to the number of nested folders before going to the data folder. Each depth will correspond to a type of analysis, could be a subject, session or trial in any order. 

then the complete data set will be describe using a series of nested dictionaries. Example: 
```python
depth = 3
{'subject_01': {'session_01': ['trial_01', 'trial_02', 'trial_03'],
                 'session_02': ['trial_01', 'trial_02', 'trial_03']},
 'subject_02': {'session_01': ['trial_01', 'trial_02', 'trial_03'],
                'session_02': ['trial_01', 'trial_02', 'trial_03']}} 
```
Est ce que cette structure est nécéssaire ? Au final on peut tout parcourir et recréer la meme arborescence <= 
==> Est ce qu'on integrerai pas les ID des calib à cette endroit là. 

From there it will be easy to access all the data just using the dictionary keys, and to process automatically all the data. 

Here there will be metadata for the processing of the data set and other that would be good to have for the publication of the data set.


| name               | type      | optional | example                                                              |
| ------------------ | --------- | -------- | -------------------------------------------------------------------- |
| data_set_name      | str       |          | 'my_data_set'                                                        |
| data_set_version   | str       |          | '1.0'                                                                |
| folder_depth       | int       |          | 3                                                                    |
| forlder_roles      | list[str] |          | ['subject', 'session', 'trial']                                      |
| data_set_structure | dict      |          | {'subject_01': {'session_01': ['trial_01', 'trial_02', 'trial_03']}} |
| license            | str       |          | 'CC-BY-NC'                                                           |
| creator            | dict      |          | {name="bitex", orcid="…", email="…"}                                 |
| description        | str       |          | 'This is a test data set'                                            |

## Metadata video 
These metadata are common to all the video files (00_calibration_data, 01_data_video)

### Metadata processing

| name              | type      | optional | examplel                |
| ----------------- | --------- | -------- | ----------------------- |
| camera_ID         | str       |          | 'camera_01'             |
| camera_model      | str       |          | 'GoPro Hero 7'          |
| resolution        | list[int] |          | [widthxheight] in pixel |
| frame_rate        | int       |          | 30                      |
| frame_count       | int       |          | 1000                    |
| compression_codec | str       |          | 'H.264'                 |
| video_format      | str       |          | 'mp4'                   |
| lossless          | bool      |          | True or False           |
| distorsion_model  | str       |          | 'pinhole'/'fisheye'     |
| undistort         | bool      |          | True or False           |

### Metadata camera hardware (but that could be good practice to have them for example in the case of publications)
These metadata might not be always necessary but could be a good practice to have them.

| name              | type        | optional | Example              |
| ----------------- | ----------- | -------- | -------------------- |
| focal_lenght_mm   | float       |          | 4.5                  |
| aperture_f_number | float 2.8   |          |                      |
| sensor_size_mm    | list[float] |          | [widthxheight] in mm |
| shutter_speed     | float       |          | 1/60                 |
| iso               | int         |          | 100                  |
| gain_db           | float       |          | 0.0                  |
| white_balance     | str         |          | 'auto'               |
| lens_type         | str         |          | 'wide'               |

## 00_calibration_data
cf metadata video plus some information about the calibration process.

| name                   | type | optional | Example                                                               |
| ---------------------- | ---- | -------- | --------------------------------------------------------------------- |
| calibration_type       | str  |          | 'charuco/scene/'                                                      |
| calibration_parameters | dict |          | {'board_type': 'charuco', 'board_size': [5, 7], 'square_size': 0.025} |

**calibration_paramters** : 
  - **board_type** : Type of board used for the calibration. It can be 'charuco', 'chessboard' or 'circle grid'.
  - **board_size** : Size of the board used for the calibration. It can be a list of 2 values [width, height] in number of squares.
  - **square_size** : Size of the square used for the calibration. It can be a float value in meters.
  - **circle_diameter** : Size of the circle used for the calibration. It can be a float value in meters.
  - **scene** : If the calibration is done on a scene or not.
  - **scene_points_positions** : array of 3D points in the world coordinate system. It can be a list of 3D points in meters.


## 01_data_video
cf metadata video.
In the metadata here we will indicated the ID of the intrinsics and extrinsics calibration folder (they can be from different session). However as our philosophy is to have everything in each folder the calibration matrix should be here too. The fact to have the pointing to the extrinsics and intrinsics folder is to be able to reprocess the extrinisc and intrinsics matrix if necessary. 
Here the calib_matrix is not considered as a metadata, but as an individual data in a toml file. These data will be integrated into the HDF5 file in the following step as a specific metadata. 


## 02_keypoints_2D_multisubject
All this metadata are integrated into the HDF5 file.

### Metadata calibration matrix

| name        | type                         | Optional | Example      |
| ----------- | ---------------------------- | -------- | ------------ |
| size        | list[int]                    |          | [1920, 1080] |
| matrix      | list[list[float]] 3x3 matrix |          |              |
| distortions | list[float]                  |          |              |
| rotation    | list[float]                  |          |              |
| translation | list[float]                  |          |              |
| fisheye     | bool                         |          | True/False   |

#### description of the metadata
- **size** : Correspond to the size of the image used 
- **Matrix** : example [[ 1237.0284339307132, 0.0, 951.8049061116914], [ 0.0, 1239.2592485704645, 528.5344016190471], [ 0.0, 0.0, 1.0]]
- **distortions** : Correspond to the distorsion coefficients used in the camera calibration. it can be a list of 5 or 14 values depending on the model used. example :  [ -0.13060079196920754, 0.13400864981470154, 0.00018961778799407008, -0.0010206183805026338]
- **rotation** : example [ 0.9618561526427922, 1.5294483485830126, 0.5242193500626191] : rotation of the camera in the world coordinate system.
- **translation** : example [ -0.6270525677142821, -0.486046176284735, 1.664813522720072] : translation of the camera in the world coordinate system.
- **fisheye** : If the camera is a fisheye camera or not. If True, we should have a metadata for the fisheye in the HDF5 file.
==> What are the metadata to add for the fisheye camera ?

### Metadata keypoint detection
One of the problem here is that we have to deal with different methods used to detect the keypoints.The methods used does not have the same parameters. As a result we should only propose a common structure for the metadata to incite people to structure their metadata and to give them. 

| name              | type | optional | Example                |
| ----------------- | ---- | -------- | ---------------------- |
| methode_name      | str  |          | 'OpenPose'             |
| method_parameters | dict |          | ....                   |
| bbox              | bool |          | True                   |
| keypoints_indice  | dict |          | {'nose': 0, 'neck': 1} |
| frame_per_second  | int  |          | 30                     |

#### description of the metadata
- **methode_name** : Name of the method used to detect the keypoints. It can be 'OpenPose', 'AlphaPose', 'PoseNet', 'RTMpose2D' or any other method used.
- **method_parameters** : A dictionary of parameters used to obtain the 2D keypoints. These parameters are specific to the method used and will be difficult to define a common structure. Some examples:
  - threshold: float 0.5 : threshold used to filter the keypoints.
  - bbox: bool True : if the method used detect the bbox or not.
  - data_set_training: str 'COCO' : data set used to train the model if available.
- **bbox** : If the method used detect the bbox or not.
- **keypoints_indice** : A dictionary of indices to name (or the opposite) to have the name of the keypoints in the array in the HDF5 file. 
- **frame_per_second** : The frame rate of the processed point (can be different from the original video).
  
### Metadata video
Probably all the former metada field from the 01_data_video should be integrated into the HDF5 file here under the 'video' group.

### Metadata data_set
Do we want here to know the name of the subject,session and trial ==> yes, but should only be for the subject name an ID of the subjects.(Any information about the subject should be in a separated file). Could look like this:

## 03_keypoints_2D_monosubject
Should we integrate all the metadata from the 02_keypoints_2D_multisubject. 


## 04_keypoints_3D_monosubject

We use in it the same metadata as in the 02_keypoints_2D_multisubject if from_2D_data is True.
If from_2D_data is False, we only used the metadata from the 01_data_video.

### Metadata triangulation or Method used to obtain the 3D keypoints
| name               | type   | optional | Example                     |
| ------------------ | ------ | -------- | --------------------------- |
| from_2D_data       | bool   |          | True                        |
| methode_name       | str    |          | 'weighted DDLT'/'RTMpose3d' |
| methode_parameters | dict() |          | {}                          |


#### description of the metadata
- **from_2D_data** : If the 3D keypoints are obtained from triangulated 2D data or from a lifting method from 2D to 3D. If True, it is a triangulation . If False, it is a lifting method from 2D to 3D. 
- **methode_name** : Name of the method used to obtain the 3D keypoints. It can be 'weighted DDLT', 'RTMpose3d' or any other method used.
- **methode_parameters** : A dictionary of parameters used to obtain the 3D keypoints. These parameters are specific to the method used and will be difficult to define a common structure. Some examples:
  - threshold: float 0.5 : threshold used to filter the keypoints.
  - this will depend on the method used. 
