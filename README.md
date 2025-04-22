```mermaid
graph LR
    ROOT["<strong>project_root</strong>"]

    %% Fichier racine
    ROOT --> META["Metadata_data_set.hdf5"]

    %% 00 – Calibration vidéo
    ROOT --> CALIB["00_Calibration_video"]
    CALIB --> CALIB_ID["IDXX_calib"]
    CALIB_ID --> CALIB_TOML["calib_matrix.toml"]

    CALIB_ID --> CALIB_EX["Extrinseque"]
    CALIB_EX --> EX_C1["cam_01/"]
    CALIB_EX --> EX_C2["cam_02/"]
    CALIB_EX --> EX_CN["cam_n/"]

    CALIB_ID --> CALIB_IN["Intrinseque"]
    CALIB_IN --> IN_C1["cam_01/"]
    CALIB_IN --> IN_C2["cam_02/"]
    CALIB_IN --> IN_CN["cam_n/"]

    %% 01 – Vidéos brutes
    ROOT --> DATA["01_Data_video"]
    DATA --> DATA_SUB["Sujet_XX"]
    DATA_SUB --> DATA_SESS["Session_XX"]
    DATA_SESS --> DATA_TASK["Tache_XX"]
    DATA_TASK --> DATA_CALMAT["calib_mat.toml"]
    DATA_TASK --> DATA_CAM["cam_XX"]
    DATA_CAM --> DATA_AVI["cam_XX.avi"]

    %% 02 – Keypoints 2D multi‑subject
    ROOT --> KP2D_MULTI["02_Keypoints2D_multisubject"]
    KP2D_MULTI --> KP2D_METH["Methode_XX"]
    KP2D_METH --> KP2D_SUB["Sujet_XX"]
    KP2D_SUB --> KP2D_SESS["Session_XX"]
    KP2D_SESS --> KP2D_TASK["Tache_XX"]
    KP2D_TASK --> KP2D_FILE["Data_multi_person.hdf5"]

    %% 03 – Keypoints 2D mono‑subject
    ROOT --> KP2D_MONO["03_Keypoints2D_monosubject"]
    KP2D_MONO --> KP2D_MONO_METH["Methode_XX"]
    KP2D_MONO_METH --> KP2D_MONO_SUB["Sujet_XX"]
    KP2D_MONO_SUB --> KP2D_MONO_SESS["Session_XX"]
    KP2D_MONO_SESS --> KP2D_MONO_TASK["Tache_XX"]
    KP2D_MONO_TASK --> KP2D_MONO_ID["ID_subject_XX"]
    KP2D_MONO_ID --> KP2D_MONO_FILE["Data_mono_person.hdf5"]

    %% 04 – Keypoints 3D mono‑subject
    ROOT --> KP3D_MONO["04_Keypoints3D_monosubject"]
    KP3D_MONO --> KP3D_METH["Methode2D_XX_Methode_3D_XX"]
    KP3D_METH --> KP3D_SUB["Sujet_XX"]
    KP3D_SUB --> KP3D_SESS["Session_XX"]
    KP3D_SESS --> KP3D_TASK["Tache_XX"]
    KP3D_TASK --> KP3D_ID["ID_subject_XX"]
    KP3D_ID --> KP3D_C3D["Data_mono_person.c3d"]
    KP3D_ID --> KP3D_HDF5["Data_mono_person.hdf5"]

    %% 05 – Ground‑truth 3D
    ROOT --> GT3D["05_Ground_truth_3D"]
    GT3D --> GT3D_METH["Methode2D_XX_Methode_3D_XX"]
    GT3D_METH --> GT3D_SUB["Sujet_XX"]
    GT3D_SUB --> GT3D_SESS["Session_XX"]
    GT3D_SESS --> GT3D_TASK["Tache_XX"]
    GT3D_TASK --> GT3D_ID["ID_subject_XX"]
    GT3D_ID --> GT3D_CAL["calib_matrix.toml"]
    GT3D_ID --> GT3D_C3D["Data_mono_person.c3d"]
    GT3D_ID --> GT3D_HDF5["Data_mono_person.hdf5"]

    %% 06 – Ground‑truth 2D
    ROOT --> GT2D["06_Ground_truth_2D"]
    GT2D --> GT2D_METH["Methode_XX"]
    GT2D_METH --> GT2D_SUB["Sujet_XX"]
    GT2D_SUB --> GT2D_SESS["Session_XX"]
    GT2D_SESS --> GT2D_TASK["Tache_XX"]
    GT2D_TASK --> GT2D_ID["ID_subject_XX"]
    GT2D_ID --> GT2D_HDF5["Data_mono_person.hdf5"]
````


````bash
    |   Metadata_data_set.hdf5
    |
    +---00_Calibration_video
    |   \---IDXX_calib
    |       |   calib_matrix.toml
    |       |
    |       +---extrinsics
    |       |   \---cam_XX
    |       |           cam_XX.avi
    |       |
    |       \---intrinsics
    |           \---cam_XX
    |                   cam_XX.avi
    |
    +---01_Data_video
    |   \---Sujet_XX
    |       \---Session_XX
    |           \---Tache_XX
    |               |   calib_mat.toml
    |               |
    |               \---cam_XX
    |                       cam_XX.avi
    |
    +---02_Keypoints2D_multisubject
    |   \---Methode_XX
    |       \---Sujet_XX
    |           \---Session_XX
    |               \---Tache_XX
    |                       Data_multi_person.hdf5
    |
    +---03_Keypoints2D_monosubject
    |   \---Methode_XX
    |       \---Sujet_XX
    |           \---Session_XX
    |               \---Tache_XX
    |                   \---ID_subject_XX
    |                           Data_mono_person.hdf5
    |
    +---04_Keypoints3D_monosubject
    |   \---Methode2D_XX_Methode_3D_XX
    |       \---Sujet_XX
    |           \---Session_XX
    |               \---Tache_XX
    |                   \---ID_subject_XX
    |                           Data_mono_person.c3d
    |                           Data_mono_person.hdf5
    |
    +---05_Ground_truth_3D
    |   \---Methode2D_XX_Methode_3D_XX
    |       \---Sujet_XX
    |           \---Session_XX
    |               \---Tache_XX
    |                   \---ID_subject_XX
    |                           calib_matrix.toml
    |                           Data_mono_person.c3d
    |                           Data_mono_person.hdf5
    |
    \---06_Ground_truth_2D
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
In this organisation it seems that a lot of data are duplicated. The main purpose here is to allow each "atomic" folder to be processed by itself. Also each part can be easily shared with other people. Indeed, you could want to share only the 2D data with someone else. In this case, you will just have to share the 02_Keypoints2D_multisubject folder without having to share the 01_Data_video folder or do any annoying copy and paste. 


## 00_Calibration video
Still a discussion to have on the codec and the video format that should be used. Globally, it seems that we want lossless compression with the best quality possible.

h.264 ? Is it lossless ?
h.265 ? Is it lossless ?



## 01_Data_video
Same question as before. What codec should be used ==> contact teams from INRIA or from politechnique Montreal. 

The calib_mat.toml file is the calibration matrix that will be used to calibrate the video. It is a toml file that contains the information about the camera and the calibration matrix intrinsics and extrinsics.


## 02_Keypoints2D_multisubject
