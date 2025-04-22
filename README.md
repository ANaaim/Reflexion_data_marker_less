```mermaid
graph TD
    %% Racine
    ROOT["<strong>project_root</strong>"]

    %% 00 ────────── Calibration vidéo
    ROOT --> CALIB["00_Calibration_video"]
    CALIB --> CALIB_ID["IDXX_calib"]

    CALIB_ID --> CALIB_EX["Extrinseque"]
    CALIB_EX --> EX_C1["cam_01"]
    EX_C1 --> EX_C1_F["frames/"]
    EX_C1_F --> EX_C1_IMG1["frame_0001.jpg"]
    EX_C1_F --> EX_C1_IMG2["frame_0002.jpg"]
    EX_C1 --> EX_C1_YAML["extrinsics_cam01.yaml"]

    CALIB_EX --> EX_C2["cam_02"]
    EX_C2 --> EX_C2_YAML["extrinsics_cam02.yaml"]
    CALIB_EX --> EX_CN["cam_n"]

    CALIB_ID --> CALIB_IN["Intrinseque"]
    CALIB_IN --> IN_C1["cam_01"]
    IN_C1 --> IN_C1_YAML["intrinsics_cam01.yaml"]
    CALIB_IN --> IN_C2["cam_02"]
    IN_C2 --> IN_C2_YAML["intrinsics_cam02.yaml"]
    CALIB_IN --> IN_CN["cam_n"]

    %% 01 ────────── Vidéos brutes
    ROOT --> DATA["01_Data_video"]
    DATA --> DATA_SUB["Sujet_XX"]
    DATA_SUB --> DATA_SESS["Session_XX"]
    DATA_SESS --> DATA_TASK["Tache_XX"]
    DATA_TASK --> DATA_CAM["cam_XX"]
    DATA_CAM --> DATA_MP4["video_camXX.mp4"]
    DATA_CAM --> DATA_TIM["timestamps_camXX.csv"]

    %% 02 ────────── Keypoints 2D multi‑sujet
    ROOT --> KP2D_MULTI["02_Keypoints2D_multisubject"]
    KP2D_MULTI --> KP2D_MULTI_METH["Methode_XX"]
    KP2D_MULTI_METH --> KP2D_MULTI_SUB["Sujet_XX"]
    KP2D_MULTI_SUB --> KP2D_MULTI_SESS["Session_XX"]
    KP2D_MULTI_SESS --> KP2D_MULTI_TASK["Tache_XX"]
    KP2D_MULTI_TASK --> KP2D_MULTI_NPY["keypoints2d.npy"]
    KP2D_MULTI_TASK --> KP2D_MULTI_JSON["meta.json"]

    %% 03 ────────── Keypoints 2D mono‑sujet
    ROOT --> KP2D_MONO["03_Keypoints2D_monosubject"]
    KP2D_MONO --> KP2D_MONO_METH["Methode_XX"]
    KP2D_MONO_METH --> KP2D_MONO_SUB["Sujet_XX"]
    KP2D_MONO_SUB --> KP2D_MONO_SESS["Session_XX"]
    KP2D_MONO_SESS --> KP2D_MONO_TASK["Tache_XX"]
    KP2D_MONO_TASK --> KP2D_MONO_ID["ID_subject_XX"]
    KP2D_MONO_ID --> KP2D_MONO_NPY["keypoints2d.npy"]
    KP2D_MONO_ID --> KP2D_MONO_CSV["confidence.csv"]

    %% 04 ────────── Keypoints 3D mono‑sujet
    ROOT --> KP3D_MONO["04_Keypoints3D_monosubject"]
    KP3D_MONO --> KP3D_MONO_METH["Methode2D_XX_Methode_3D_XX"]
    KP3D_MONO_METH --> KP3D_MONO_SUB["Sujet_XX"]
    KP3D_MONO_SUB --> KP3D_MONO_SESS["Session_XX"]
    KP3D_MONO_SESS --> KP3D_MONO_TASK["Tache_XX"]
    KP3D_MONO_TASK --> KP3D_MONO_ID["ID_subject_XX"]
    KP3D_MONO_ID --> KP3D_MONO_PKL["keypoints3d.pkl"]
    KP3D_MONO_ID --> KP3D_MONO_SKT["skeleton.json"]

    %% 05 ────────── Ground‑truth 3D
    ROOT --> GT3D["05_Ground_truth_3D"]
    GT3D --> GT3D_METH["Methode2D_XX_Methode_3D_XX"]
    GT3D_METH --> GT3D_SUB["Sujet_XX"]
    GT3D_SUB --> GT3D_SESS["Session_XX"]
    GT3D_SESS --> GT3D_TASK["Tache_XX"]
    GT3D_TASK --> GT3D_ID["ID_subject_XX"]
    GT3D_ID --> GT3D_TRC["mocap.trc"]
    GT3D_ID --> GT3D_C3D["markers.c3d"]

    %% 06 ────────── Ground‑truth 2D
    ROOT --> GT2D["06_Ground_truth_2D"]
    GT2D --> GT2D_METH["Methode_XX"]
    GT2D_METH --> GT2D_SUB["Sujet_XX"]
    GT2D_SUB --> GT2D_SESS["Session_XX"]
    GT2D_SESS --> GT2D_TASK["Tache_XX"]
    GT2D_TASK --> GT2D_ID["ID_subject_XX"]
    GT2D_ID --> GT2D_NPY["keypoints2d_gt.npy"]
    GT2D_ID --> GT2D_ANN["annotations.json"]
````



````bash
│   Metadata_data_set.hdf5
│
├───00_Calibration_video
│   └───IDXX_calib
│       │   calib_matrix.toml
│       │
│       ├───Extrinseque
│       │   ├───cam_01
│       │   ├───cam_02
│       │   └───cam_n
│       └───Intrinseque
│           ├───cam_01
│           ├───cam_02
│           └───cam_n
├───01_Data_video
│   └───Sujet_XX
│       └───Session_XX
│           └───Tache_XX
│               │   calib_mat.toml
│               │
│               └───cam_XX
│                       cam_XX.avi
│
├───02_Keypoints2D_multisubject
│   └───Methode_XX
│       └───Sujet_XX
│           └───Session_XX
│               └───Tache_XX
│                       Data_multi_person.hdf5
│
├───03_Keypoints2D_monosubject
│   └───Methode_XX
│       └───Sujet_XX
│           └───Session_XX
│               └───Tache_XX
│                   └───ID_subject_XX
│                           Data_mono_person.hdf5
│
├───04_Keypoints3D_monosubject
│   └───Methode2D_XX_Methode_3D_XX
│       └───Sujet_XX
│           └───Session_XX
│               └───Tache_XX
│                   └───ID_subject_XX
│                           Data_mono_person.c3d
│                           Data_mono_person.hdf5
│
├───05_Ground_truth_3D
│   └───Methode2D_XX_Methode_3D_XX
│       └───Sujet_XX
│           └───Session_XX
│               └───Tache_XX
│                   └───ID_subject_XX
│                           Data_mono_person.c3d
│                           Data_mono_person.hdf5
│
└───06_Ground_truth_2D
    └───Methode_XX
        └───Sujet_XX
            └───Session_XX
                └───Tache_XX
                    └───ID_subject_XX
                            Data_mono_person.hdf5
````
