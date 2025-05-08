## data structure
````mermaid
graph LR
    ROOT["<strong>project_root</strong>"]

    %% Global metadata
    ROOT --> META["Metadata_data_set.toml"]

    %% 00 – Calibration data
    ROOT --> CALIB["00_calibration_data"]
    CALIB --> CALIB_ID["IDXX_calib"]
    CALIB_ID --> CALIB_MAT["calib_matrix.toml"]
    CALIB_ID --> CALIB_META["metadata_video.toml"]
    CALIB_ID --> EX["extrinsics"]
    EX --> EX_CAM["cam_XX/"]
    EX_CAM --> EX_AVI["cam_XX.avi"]
    CALIB_ID --> IN["intrinsics"]
    IN --> IN_CAM["cam_XX/"]
    IN_CAM --> IN_AVI["cam_XX.avi"]

    %% 01 – Data video
    ROOT --> VIDEO["01_data_video"]
    VIDEO --> V_SUB["subject_XX"]
    V_SUB --> V_SESS["session_XX"]
    V_SESS --> V_TRIAL["trial_XX"]
    V_TRIAL --> V_TRIAL_CAL["calib_mat.toml"]
    V_TRIAL --> V_TRIAL_META["metadata_video.toml"]
    V_TRIAL --> RAW["raw"]
    RAW --> RAW_CAM["cam_XX/"]
    RAW_CAM --> RAW_AVI["cam_XX.avi"]
    V_TRIAL --> UND["undistorted"]
    UND --> UND_CAM["cam_XX/"]
    UND_CAM --> UND_AVI["cam_XX.avi"]

    %% 02 – Keypoints 2D multisubject
    ROOT --> K2D_MULTI["02_keypoints_2D_multisubject"]
    K2D_MULTI --> K2D_METHOD["method_XX"]
    K2D_METHOD --> K2D_SUB["subject_XX"]
    K2D_SUB --> K2D_SESS["session_XX"]
    K2D_SESS --> K2D_TRIAL["trial_XX"]
    K2D_TRIAL --> K2D_FILE["data_multi_person.hdf5"]

    %% 03 – Keypoints 2D monosubject
    ROOT --> K2D_MONO["03_keypoints_2D_monosubject"]
    K2D_MONO --> K2D_MONO_METHOD["method_XX"]
    K2D_MONO_METHOD --> K2D_MONO_SUB["subject_XX"]
    K2D_MONO_SUB --> K2D_MONO_SESS["session_XX"]
    K2D_MONO_SESS --> K2D_MONO_TRIAL["trial_XX"]
    K2D_MONO_TRIAL --> K2D_MONO_ID["ID_subject_XX"]
    K2D_MONO_ID --> K2D_MONO_FILE["data_mono_person.hdf5"]

    %% 04 – Keypoints 3D monosubject
    ROOT --> K3D_MONO["04_keypoints_3D_monosubject"]
    K3D_MONO --> K3D_METHOD["method_2D_XX_method_3D_XX"]
    K3D_METHOD --> K3D_SUB["subject_XX"]
    K3D_SUB --> K3D_SESS["session_XX"]
    K3D_SESS --> K3D_TRIAL["trial_XX"]
    K3D_TRIAL --> K3D_ID["ID_subject_XX"]
    K3D_ID --> K3D_C3D["data_mono_person.c3d"]
    K3D_ID --> K3D_HDF5["data_mono_person.hdf5"]

    %% 05 – Ground truth 3D
    ROOT --> GT3D["05_ground_truth_3D"]
    GT3D --> GT3D_SUB["subject_XX"]
    GT3D_SUB --> GT3D_SESS["session_XX"]
    GT3D_SESS --> GT3D_TRIAL["trial_XX"]
    GT3D_TRIAL --> GT3D_CAL["calib_matrix.toml"]
    GT3D_TRIAL --> GT3D_C3D["data_mono_person.c3d"]
    GT3D_TRIAL --> GT3D_HDF5["data_mono_person.hdf5"]

    %% 06 – Ground truth keypoints 2D
    ROOT --> GT2D["06_ground_truth_keypoints_2D"]
    GT2D --> GT2D_SUB["subject_XX"]
    GT2D_SUB --> GT2D_SESS["session_XX"]
    GT2D_SESS --> GT2D_TRIAL["trial_XX"]
    GT2D_TRIAL --> GT2D_ID["ID_subject_XX"]
    GT2D_ID --> GT2D_HDF5["data_mono_person.hdf5"]
````

## Discussion AM : flexibilité sur les noms 
    - metadata à mettre  dans calibration data
    - est ce qu'il ne faudrait pas avoir un dossier spécifique pour la calibration (le fichier de sortie parceque c'est une sortie)
    - Le dossier ne correspond pas forcément à une fléche d'entrée dans nos fonctions. 
    - Est ce que l'on ne parlerait pas plutot de "type de données" plutot que d'avoir des noms spécifiques.
        - video calibration
        - calculated calibration
        - video data
        - keypoints 2D multisubject
        - keypoints 2D monosubject
        - keypoints 3D monosubject
        - ground truth 3D <=== Au final ground truth 3D et 2D sont les mêmes données que keypoints 3D et 2D. Est ce que l'on doit les garder ? 
        - ground truth 2D
        ==> donc prévoir de rajouter metadata pour chaque type de données permettant d'identifier ground truthe or not. 
````bash
    | 
    +---Metadata_data_set.toml
    |
    +---00_calibration_data
    |   \---IDXX_calib
    |       |   calib_matrix.toml
    |       |   metadata_video_calibration .toml
    |       |
    |       +---extrinsics
    |       |   \---cam_XX
    |       |           cam_XX.avi
    |       |
    |       \---intrinsics
    |           \---cam_XX
    |                   cam_XX.avi
    |
    +---01_calibration_matrix
    |      \---subject_XX
    |          \---session_XX
    |              \---trial_XX
    |                      |
    |                      \---calib.toml
    |
    +---02_data_video <== 
    |   +---subject_XX
    |   |    +---session_XX
    |   |        +---trial_XX
    |   ...         |   optional : calib_mat_from_IDXX.toml 
    |               |   metadata_video.toml
    |               \---video
    |                    \---cam_XX
    |                        cam_XX.avi/cam.jpg 
    |
    +---03_keypoints_2D_multisubject 
    |   |   +---metadata_method_origin_data  <== C'est sur que c'est nécéssaire (pas forcément standardisé
    |   |   +---subject_XX
    |   |       +---session_XX
    |   |           +---trial_XX
    |   |                   data_multisubject.hdf5
    |   ..
    |
    +---04_keypoints_2D_monosubject
    |   |   +---metadata_method_origin_data
    |   |   +---subject_XX
    |   |       +---session_XX
    |   |           +---trial_XX
    |   |                +---ID_subject_XX
    |   |                        data_monosubject.hdf5
    |   ...
    |   
    \---05_keypoints_3D_monosubject
       |---metadata_method_origin_data
       +---subject_XX
            +---session_XX
                +---trial_XX
                    +---ID_subject_XX
                        data_mono_person.c3d
                        data_mono_person.hdf5
````

## Global comment

### Structure of the data
The part in the structure where we found subject_XX, session_XX and trial_XX is not suposed to be always there. The most important element is that each final folder can be processed by itself. The information about the subject session and trial is not mandatory. It is just a way to organize the data that will be indicated in the metadata_dataset.h5 file. This information will be contained in different information fields : **folder_depth** and different nested dictionaries. The **folder_depth** will correspond to the number of folders that are in the path. In our example, the first folder will be the subject, the second one the session and the third one the trial which will correspond to a **folder_depth** of 3.
After a nested dictionary will be created to store the information about the subject, session and trial allowing the user to access the information easily.

**folder_depth** might not be the best name for this, as **depth** might be enough and largely used in the community. However, as we can use RGB-D camera precising that we are talking about the folder might avoid some confusion. 

### Philosophy of the data organisation 
In this organisation it seems that a lot of data are duplicated. The main purpose here is to allow each leaf folder to be processed by itself. Also each part can be easily shared with other people. Indeed, you could want to share only the 2D data with someone else. In this case, you will just have to share the 02_keypoints_2D_multisubject folder without having to share the 01_data_video folder or do any annoying copy and paste.

A proposition to avoid data duplication (I think both should be done) should be to consider that we have some type of data instead : 
    - calibration video data
    - calibration of the video
    - video data
    - keypoints 2D multisubject
    - keypoints 2D monosubject
    - keypoints 3D monosubject

In the end, ground truth 3D and 2D are the same data as keypoints 3D and 2D. So we could consider that we have only one type of data. This would be a more general way to consider the data, but it might be less clear for the user.

This approach allow a more "object" approach for the code : you give a folder of calibration and a folder of 2D_keypoints and the code will be able to process it. But you do not need the calibration to process the video data to obtain the 2D keypoints. As a result, the second approach is more flexible and allow to process the data in a more modular way. However, we loose the aspect of the each leaf folder being able to be processed by itself.

### File format 

It has been chosen to use for metadata to use the toml format. The toml format is a format that is easy to read and write. It is also easy to parse and generate. However, for storing the data that are in array format, it has been chosen to use the hdf5 format. The hdf5 format is a format that is very used in the scientific community and that is easy to read and write. The hdf5 format is a binary format that is very efficient for storing large amount of data. It is also easy to compress and decompress. It has been chosen when hdf5 format is used to store the metadata in the hdf5 file to be sure that the metadata is available with the data and allow the hdf5 to be easily shared and processed on their own. 

## 00_calibration video

### Video format
Still a discussion to have on the codec and the video format that should be used. Globally, it seems that we want lossless compression with the best quality possible.

The organisation where in the leaf folder we have the video file inside a folder with the name of the camera is keeped as sometime the data can be exported as images and necessitate to have a folder to store the images in a clean manner. In addition, some software such as theia are using such a structure to store the data.

These should be a recommendation on the codec and the video format that should be used. Indeed, sometime format are determined by the camera used. However, having guideline might be interesting for user not used to that kind of question such as in the biomechanics community. The codec should be a lossless codec and the video format should be a format that is easy to read and write.

Two propositions (to validate with people from computer vision background) :

| Codec          | Format                                                              |
| -------------- | ------------------------------------------------------------------- |
| FFV1           | .mkv / .avi                                                         |
| H.264 lossless | .mp4 / .mkv (Use -qp 0 with libx264, but not always byte-identical) |


### The metadata_video.toml file
See in the file description_metadata.md. 

## 01_calibration_data

### The calib_mat.toml file 
Currently this is the file format that LBMC is using due to P2S. Other format could be used if more convenient. But translator should be added to convert the data from one format to another.

It should be decided which is the more common format that should be used for describing the camera intrinsics and extrinsics matrix. Currently, it seems that it is the OpenCV format that is used which would seems one of the most common format. 


````toml
[cam_01]
name = "M11139"
size = [ 1920.0, 1080.0]
matrix = [ [ 1237.0284339307132, 0.0, 951.8049061116914], [ 0.0, 1239.2592485704645, 528.5344016190471], [ 0.0, 0.0, 1.0]]
distortions = [ -0.13060079196920754, 0.13400864981470154, 0.00018961778799407008, -0.0010206183805026338]
rotation = [ 0.9618561526427922, 1.5294483485830126, 0.5242193500626191]
translation = [ -0.6270525677142821, -0.486046176284735, 1.664813522720072]
fisheye = false
...
[cam_XX]
name = "cam_XX"
size = [ 1920.0, 1080.0]
matrix = [ [ 2144.9601175685557, 0.0, 967.859729904023], [ 0.0, 2156.5146368800865, 550.1781128465403], [ 0.0, 0.0, 1.0]]
distortions = [ -0.09658154939711396, 0.11563585511085413, 0.0008519503575105665, 0.0028026774341342975]
rotation = [ -0.8305202999790112, -1.891827398628365, 0.34774377648736793]
translation = [ 0.28359105437278004, -0.611816310511053, 3.1709460450221942]
fisheye = false
....

[metadata]
ID_calibration = "ID_XX"
calibration_type = "charuco/scene/"
calibration_parameters = {board_type = "charuco", board_size = [5, 7], square_size = 0.025}
board_type = "charuco"
board_size = [5, 7]
square_size = 0.025
circle_diameter = 0.025
scene = true
scene_points_positions = [ [0.0, 0.0, 0.0], [0.0, 0.0, 1.0], [0.0, 1.0, 1.0], [1.0, 1.0, 1.0], [1.0, 1.0, 2.0] ]
````


## 02_data_video
The video organisation is similar to the one used in the 00_calibration_data.

Somes questions to be asked here :
    - Is it expected that the video will be in raw and undistorted format ? 
    - Should the video be reorientation there. 
    
The calib_mat.toml file is the calibration matrix that will be used to calibrate the video. It is a toml file that contains the information about the camera and the calibration matrix intrinsics and extrinsics.


## 03_keypoints_2D_multisubject

### data_multi_person.hdf5
These data should contains the different bbox detected and the keypoints detected. The bbox should be in the format [x1, y1, x2, y2] where x1 and y1 are the coordinates of the top left corner and x2 and y2 are the coordinates of the bottom right corner. The keypoints should be in the format [x, y, conf] where x and y are the coordinates of the keypoint and conf is the confidence of the detection.

````bash
 data_multi_person.hdf5
    |
    +--- cam_01
    |       |--- frame_01
    |       |       |---Id_XX
    |       |        ...    |---bbox ==> 4x1 int array [x1,y1,x2,y2] if no bbox detected [NaN, NaN NaN, NaN]
    |       |               \---keypoints ==> 3xNb_Keypoint float [x,y,confidence] if no keypoint detected [NaN, NaN,NaN]
    |       ...
    |       \+--- frame_XX
    |              |---Id_XX
    |               ...    |---bbox ==> 4x1 int array [x1,y1,x2,y2] if no bbox detected [NaN, NaN NaN, NaN]
    |                      \---keypoints ==> 3xNb_Keypoint float array [x,y,confidence] if no keypoint detected [NaN, NaN,NaN]                    
    ...
    +--- cam_XX
    |       |--- frame_01
    |       |       |---Id_XX
    |       |        ...    |---bbox ==> 4x1 int array [x1,y1,x2,y2] if no bbox detected [NaN, NaN, NaN, NaN]
    |       |               \---keypoints ==> 3xNb_Keypoint float [x,y,confidence] if no keypoint detected [NaN, NaN,NaN]
    |       ...
    |       \+--- frame_XX
    |              |---Id_XX
    |               ...    |---bbox ==> 4x1 int array [x1,y1,x2,y2] if no bbox detected [NaN, NaN NaN, NaN]
    |                      \---keypoints ==> 3xNb_Keypoint float array [x,y,confidence] if no keypoint detected [NaN,NaN,NaN]  
    |
    \--- metadata
            |---dictionary name point to their corresponding indices in the keypoints array
            \---TODO list metadata
````

## 04_keypoints_2D_monosubject

The data should be in the same format as the one used in the 02_Keypoints2D_multisubject. The only difference is that there is only one subject and one camera. 

````bash
    |   data_mono_person.hdf5
    +--- cam_01
    |       |--- bbox ==> 4xnb_frame int array [x1,y1,x2,y2,n] if no bbox detected [NaN, NaN NaN, NaN,i]
    |       \--- keypoints ==> 3xNb_Keypointxnb_frame float [x,y,confidence,n] if no keypoint detected [NaN, NaN,NaN,i]
    |
    ...
    +--- cam_XX
    |       |--- bbox ==> 4xnb_frame int array [x1,y1,x2,y2,n] if no bbox detected [NaN, NaN NaN, NaN,i]
    |       \--- keypoints ==> 3xNb_Keypointxnb_frame float [x,y,confidence,n] if no keypoint detected [NaN, NaN,NaN,i]
    |
    |
    \---metadata
            |---Calib_matrix
            |---dictionary name point to their corresponding indices in the keypoints array
            |---TODO list metadata cf description_metadata.md
            \---TODO list metadata cf description_metadata.md
````

## 05_keypoints_3D_monosubject

One question here is on the use of which data format. The c3d format is a format very used in biomechanics which might make it harder to be used by other people from other domains such as computer vision. The hdf5 format is more general and could be used by other people. However, there is a some useful tool to visualize the c3d data which could be useful for the user.

One solution could be to use the more general hdf5 format and to add a converter to convert the data from one format to another using a toolbox such as ezc3d. 

````bash
data_mono_person.hdf5
    |
    +---points_3D ==> 3xNb_Keypointxnb_frame float [x,y,z]
    | 
    \---metadata
            |---dictionary name point to their corresponding indices in the keypoints array
            |---TODO list metadata cf description_metadata.md
            \---TODO list metadata cf description_metadata.md
````
Similar meta data should be contained in the metadata of the c3d file. 

## 05-06 Ground_truth_3D and 2D : No more needed if we consider the approach of the data organisation as a type of data.

The data here should have the same format as the one used in the Keypoints_3D subject and Keypoints_2D subject.
The only difference is that in the 3D data the calibration matrix should be included in each leaf folder and do not have a specific folder for the method used to obtain the 3D keypoints. 

An alternative proposition could be to integrate the ground truth data as a part of the 03_keypoints_2D_monosubject and 04_keypoints_3D_monosubject with a different method name being "ground_truth_2D" and "ground_truth_3D".
This would allow to have all the data in the same place and to be able to process them together.

Both make sens and should be discussed with the team.