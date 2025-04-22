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


````mermaid

```mermaid
graph LR
    C___0["C:."]
    C___0 --> Metadata_data_set_hdf5_1
    Metadata_data_set_hdf5_1["Metadata_data_set.hdf5"]
    C___0 --> _2
    _2[""]
    00_Calibration_video_3["00_Calibration_video"]
    00_Calibration_video_3 --> IDXX_calib_4
    IDXX_calib_4["IDXX_calib"]
    IDXX_calib_4 --> calib_matrix_toml_5
    calib_matrix_toml_5["calib_matrix.toml"]
    calib_matrix_toml_5 --> _6
    _6[""]
    IDXX_calib_4 --> extrinsics_7
    extrinsics_7["extrinsics"]
    extrinsics_7 --> cam_XX_8
    cam_XX_8["cam_XX"]
    cam_XX_8 --> cam_XX_avi_9
    cam_XX_avi_9["cam_XX.avi"]
    cam_XX_avi_9 --> _10
    _10[""]
    IDXX_calib_4 --> intrinsics_11
    intrinsics_11["intrinsics"]
    intrinsics_11 --> cam_XX_12
    cam_XX_12["cam_XX"]
    cam_XX_12 --> cam_XX_avi_13
    cam_XX_avi_13["cam_XX.avi"]
    cam_XX_avi_13 --> _14
    _14[""]
    01_Data_video_15["01_Data_video"]
    01_Data_video_15 --> Sujet_XX_16
    Sujet_XX_16["Sujet_XX"]
    Sujet_XX_16 --> Session_XX_17
    Session_XX_17["Session_XX"]
    Session_XX_17 --> Tache_XX_18
    Tache_XX_18["Tache_XX"]
    Tache_XX_18 --> calib_mat_toml_19
    calib_mat_toml_19["calib_mat.toml"]
    calib_mat_toml_19 --> _20
    _20[""]
    Tache_XX_18 --> cam_XX_21
    cam_XX_21["cam_XX"]
    cam_XX_21 --> cam_XX_avi_22
    cam_XX_avi_22["cam_XX.avi"]
    cam_XX_avi_22 --> _23
    _23[""]
    02_Keypoints2D_multisubject_24["02_Keypoints2D_multisubject"]
    02_Keypoints2D_multisubject_24 --> Methode_XX_25
    Methode_XX_25["Methode_XX"]
    Methode_XX_25 --> Sujet_XX_26
    Sujet_XX_26["Sujet_XX"]
    Sujet_XX_26 --> Session_XX_27
    Session_XX_27["Session_XX"]
    Session_XX_27 --> Tache_XX_28
    Tache_XX_28["Tache_XX"]
    Tache_XX_28 --> Data_multi_person_hdf5_29
    Data_multi_person_hdf5_29["Data_multi_person.hdf5"]
    Data_multi_person_hdf5_29 --> _30
    _30[""]
    03_Keypoints2D_monosubject_31["03_Keypoints2D_monosubject"]
    03_Keypoints2D_monosubject_31 --> Methode_XX_32
    Methode_XX_32["Methode_XX"]
    Methode_XX_32 --> Sujet_XX_33
    Sujet_XX_33["Sujet_XX"]
    Sujet_XX_33 --> Session_XX_34
    Session_XX_34["Session_XX"]
    Session_XX_34 --> Tache_XX_35
    Tache_XX_35["Tache_XX"]
    Tache_XX_35 --> ID_subject_XX_36
    ID_subject_XX_36["ID_subject_XX"]
    ID_subject_XX_36 --> Data_mono_person_hdf5_37
    Data_mono_person_hdf5_37["Data_mono_person.hdf5"]
    Data_mono_person_hdf5_37 --> _38
    _38[""]
    04_Keypoints3D_monosubject_39["04_Keypoints3D_monosubject"]
    04_Keypoints3D_monosubject_39 --> Methode2D_XX_Methode_3D_XX_40
    Methode2D_XX_Methode_3D_XX_40["Methode2D_XX_Methode_3D_XX"]
    Methode2D_XX_Methode_3D_XX_40 --> Sujet_XX_41
    Sujet_XX_41["Sujet_XX"]
    Sujet_XX_41 --> Session_XX_42
    Session_XX_42["Session_XX"]
    Session_XX_42 --> Tache_XX_43
    Tache_XX_43["Tache_XX"]
    Tache_XX_43 --> ID_subject_XX_44
    ID_subject_XX_44["ID_subject_XX"]
    ID_subject_XX_44 --> Data_mono_person_c3d_45
    Data_mono_person_c3d_45["Data_mono_person.c3d"]
    Data_mono_person_c3d_45 --> Data_mono_person_hdf5_46
    Data_mono_person_hdf5_46["Data_mono_person.hdf5"]
    Data_mono_person_c3d_45 --> _47
    _47[""]
    05_Ground_truth_3D_48["05_Ground_truth_3D"]
    05_Ground_truth_3D_48 --> Methode2D_XX_Methode_3D_XX_49
    Methode2D_XX_Methode_3D_XX_49["Methode2D_XX_Methode_3D_XX"]
    Methode2D_XX_Methode_3D_XX_49 --> Sujet_XX_50
    Sujet_XX_50["Sujet_XX"]
    Sujet_XX_50 --> Session_XX_51
    Session_XX_51["Session_XX"]
    Session_XX_51 --> Tache_XX_52
    Tache_XX_52["Tache_XX"]
    Tache_XX_52 --> ID_subject_XX_53
    ID_subject_XX_53["ID_subject_XX"]
    ID_subject_XX_53 --> calib_matrix_toml_54
    calib_matrix_toml_54["calib_matrix.toml"]
    calib_matrix_toml_54 --> Data_mono_person_c3d_55
    Data_mono_person_c3d_55["Data_mono_person.c3d"]
    calib_matrix_toml_54 --> Data_mono_person_hdf5_56
    Data_mono_person_hdf5_56["Data_mono_person.hdf5"]
    calib_matrix_toml_54 --> _57
    _57[""]
    06_Ground_truth_2D_58["06_Ground_truth_2D"]
    06_Ground_truth_2D_58 --> Methode_XX_59
    Methode_XX_59["Methode_XX"]
    Methode_XX_59 --> Sujet_XX_60
    Sujet_XX_60["Sujet_XX"]
    Sujet_XX_60 --> Session_XX_61
    Session_XX_61["Session_XX"]
    Session_XX_61 --> Tache_XX_62
    Tache_XX_62["Tache_XX"]
    Tache_XX_62 --> ID_subject_XX_63
    ID_subject_XX_63["ID_subject_XX"]
    ID_subject_XX_63 --> Data_mono_person_hdf5_64
    Data_mono_person_hdf5_64["Data_mono_person.hdf5"]
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
│                           calib_matrix.toml
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

## 00_Calibration video

## 01_Data_video

## 02_Keypoints2D_multisubject
