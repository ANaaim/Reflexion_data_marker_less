````bash
    |   Metadata_data_set.toml
    |
    +---00_calibration_data
    |   \---IDXX_calib
    |       |   calib_matrix.toml
    |       |   metadata_video.toml
    |       |
    |       +---extrinsics
    |       |   \---cam_XX
    |       |           cam_XX.avi
    |       |
    |       \---intrinsics
    |           \---cam_XX
    |                   cam_XX.avi
    |
    +---01_data_video
    |   \---Sujet_XX
    |       \---Session_XX
    |           \---Tache_XX
    |               |   calib_mat.toml
    |               |   metadata_video.toml
    |               |
    |               \---cam_XX
    |                       cam_XX.avi
    |
    +---02_keypoints_2D_multisubject
    |   \---Methode_XX
    |       \---Sujet_XX
    |           \---Session_XX
    |               \---Tache_XX
    |                       Data_multi_person.hdf5
    |
    +---03_keypoints_2D_monosubject
    |   \---Methode_XX
    |       \---Sujet_XX
    |           \---Session_XX
    |               \---Tache_XX
    |                   \---ID_subject_XX
    |                           Data_mono_person.hdf5
    |
    +---04_keypoints_3D_monosubject
    |   \---Methode2D_XX_Methode_3D_XX
    |       \---Sujet_XX
    |           \---Session_XX
    |               \---Tache_XX
    |                   \---ID_subject_XX
    |                           Data_mono_person.c3d
    |                           Data_mono_person.hdf5
    |
    +---05_ground_truth_3D
    |   \---Methode2D_XX_Methode_3D_XX
    |       \---Sujet_XX
    |           \---Session_XX
    |               \---Tache_XX
    |                   \---ID_subject_XX
    |                           calib_matrix.toml
    |                           Data_mono_person.c3d
    |                           Data_mono_person.hdf5
    |
    \---06_ground_truth_keypoints_2D
        \---Methode_XX
            \---Sujet_XX
                \---Session_XX
                    \---Tache_XX
                        \---ID_subject_XX
                                Data_mono_person.hdf5

````

## Global comment

### Structure of the data
The part in the structure where we found Sujet_XX, Session_XX and Tache_XX is not suposed to be always there. The most important element is that each final folder can be processed by itself. The information about the subject session and task is not mandatory. It is just a way to organize the data that will be indicated in the metadata_dataset.h5 file. This information will be contained in different information fields : depth and different dictionaries. 

The depth will correspond to the number of folders that are in the path. In our example, the first folder will be the subject, the second one the session and the third one the task which will correspond to the depth 3.
After a nested dictionary will be created to store the information about the subject, session and task allowing the user to access the information easily. 

### Philosophy of the data organisation 
In this organisation it seems that a lot of data are duplicated. The main purpose here is to allow each leaf folder to be processed by itself. Also each part can be easily shared with other people. Indeed, you could want to share only the 2D data with someone else. In this case, you will just have to share the 02_Keypoints2D_multisubject folder without having to share the 01_Data_video folder or do any annoying copy and paste. 

### File format 

It has been chosen to use for metadata without array to use the toml format. The toml format is a format that is easy to read and write. It is also easy to parse and generate. However, for the data that are in array format, it has been chosen to use the hdf5 format. The hdf5 format is a format that is very used in the scientific community and that is easy to read and write. It is also easy to parse and generate.

This could be done also for the metadata with the data in hdf5, but it seems that it is better that the metadat to be embedded in the hdf5 file. As a result, it will be impossible to have the data without the metadata, as a result it should be always possible to process it and the file will be easier to share.

## 00_calibration video
Still a discussion to have on the codec and the video format that should be used. Globally, it seems that we want lossless compression with the best quality possible.

h.264 ? Is it lossless ?
h.265 ? Is it lossless ?



## 01_data_video
Same question as before. What codec should be used ==> contact teams from INRIA or from politechnique Montreal. 

The calib_mat.toml file is the calibration matrix that will be used to calibrate the video. It is a toml file that contains the information about the camera and the calibration matrix intrinsics and extrinsics.

### The calib_mat.toml file 
Currently this is the file format that Alex is using due to P2S. Other format could be used if more convenient. But translator should be added to convert the data from one format to another.

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
````

## 02_keypoints_2D_multisubject

structure of the Data_multi_person.hdf5
These data should contains the different bbox detected and the keypoints detected. The bbox should be in the format [x1, y1, x2, y2] where x1 and y1 are the coordinates of the top left corner and x2 and y2 are the coordinates of the bottom right corner. The keypoints should be in the format [x, y, conf] where x and y are the coordinates of the keypoint and conf is the confidence of the detection.

````bash
    |   Data_multi_person.hdf5
    +--- cam_01
    |       |--- frame_01
    |       |       |---Id_XX
    |       |               |---bbox ==> 4x1 int array [x1,y1,x2,y2] if no bbox detected [NaN, NaN NaN, NaN]
    |       |               \---keypoints ==> 3xNb_Keypoint float [x,y,confidence] if no keypoint detected [NaN, NaN,NaN]
    |       ...
    |       \+--- frame_XX
    |              |---Id_XX
    |                      |---bbox ==> 4x1 int array [x1,y1,x2,y2] if no bbox detected [NaN, NaN NaN, NaN]
    |                      \---keypoints ==> 3xNb_Keypoint float array [x,y,confidence] if no keypoint detected [NaN, NaN,NaN]                    
    ...
    +--- cam_XX
    |       |--- frame_01
    |       |       |---Id_XX
    |       |               |---bbox ==> 4x1 int array [x1,y1,x2,y2] if no bbox detected [NaN, NaN NaN, NaN]
    |       |               \---keypoints ==> 3xNb_Keypoint float [x,y,confidence] if no keypoint detected [NaN, NaN,NaN]
    |       ...
    |       \+--- frame_XX
    |              |---Id_XX
    |                      |---bbox ==> 4x1 int array [x1,y1,x2,y2] if no bbox detected [NaN, NaN NaN, NaN]
    |                      \---keypoints ==> 3xNb_Keypoint float array [x,y,confidence] if no keypoint detected [NaN, NaN,NaN]  
    |
    \---metadata
            |---dictionary name point to their corresponding indices in the keypoints array
            \---TODO list metadata
````

## 03_keypoints_2D_monosubject

The data should be in the same format as the one used in the 02_Keypoints2D_multisubject. The only difference is that there is only one subject and one camera. 

````bash
    |   Data_mono_person.hdf5
    +--- cam_01
    |      \---keypoints ==> 3xNb_Keypointxnb_frame float [x,y,confidence]
    |
    ...
    +--- cam_XX
    |      \---keypoints ==> 3xNb_Keypointxnb_frame float [x,y,confidence]
    |
    |
    \---metadata
            |---Calib_matrix
            |---dictionary name point to their corresponding indices in the keypoints array
            |---TODO list metadata
            \---TODO list metadata
````

## 04_keypoints_3D_monosubject

One question here is on the use of which data format. The c3d format is a format very used in biomechanics which might make it harder to be used by other people from other domains such as computer vision. The hdf5 format is more general and could be used by other people. However, there is a some useful tool to visualize the c3d data which could be useful for the user.

One solution could be to use the more general hdf5 format and to add a converter to convert the data from one format to another using a toolbox such as ezc3d. 

````bash
    |   Data_mono_person.hdf5
        points_3D ==> 3xNb_Keypointxnb_frame float [x,y,z]
    | 
    \---metadata
            |---dictionary name point to their corresponding indices in the keypoints array
            |---TODO list metadata
            \---TODO list metadata
````
Similar meta data should be contained in the metadata of the c3d file. 

05-06 Ground_truth_3D and 2D
The data here should have the same format as the one used in the Keypoints_3D subject and Keypoints_2D subject.
The only difference is that in the 3D data the calibration matrix should be included in each leaf folder. 