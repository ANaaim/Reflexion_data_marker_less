```mermaid
%% Arborescence projet - niveau de détail : dossiers principaux
graph TD
    ROOT["<strong>project_root</strong>"]

    %% 00 – Calibration
    ROOT --> CALIB["00_Calibration_video"]
    CALIB --> CALIB_ID["IDXX_calib"]
    CALIB_ID --> CALIB_EX["Extrinseque"]
    CALIB_EX --> CALIB_EX_C1["cam_01"]
    CALIB_EX --> CALIB_EX_C2["cam_02"]
    CALIB_EX --> CALIB_EX_CN["cam_n"]
    CALIB_ID --> CALIB_IN["Intrinseque"]
    CALIB_IN --> CALIB_IN_C1["cam_01"]
    CALIB_IN --> CALIB_IN_C2["cam_02"]
    CALIB_IN --> CALIB_IN_CN["cam_n"]

    %% 01 – Vidéos brutes
    ROOT --> DATA["01_Data_video"]
    DATA --> DATA_SUB["Sujet_XX"]
    DATA_SUB --> DATA_SESS["Session_XX"]
    DATA_SESS --> DATA_TASK["Tache_XX"]
    DATA_TASK --> DATA_CAM["cam_XX"]

    %% 02 – Keypoints 2D multi‑subject
    ROOT --> KP2D_MULTI["02_Keypoints2D_multisubject"]
    KP2D_MULTI --> KP2D_MULTI_METH["Methode_XX"]
    KP2D_MULTI_METH --> KP2D_MULTI_SUB["Sujet_XX"]
    KP2D_MULTI_SUB --> KP2D_MULTI_SESS["Session_XX"]
    KP2D_MULTI_SESS --> KP2D_MULTI_TASK["Tache_XX"]

    %% 03 – Keypoints 2D mono‑subject
    ROOT --> KP2D_MONO["03_Keypoints2D_monosubject"]
    KP2D_MONO --> KP2D_MONO_METH["Methode_XX"]
    KP2D_MONO_METH --> KP2D_MONO_SUB["Sujet_XX"]
    KP2D_MONO_SUB --> KP2D_MONO_SESS["Session_XX"]
    KP2D_MONO_SESS --> KP2D_MONO_TASK["Tache_XX"]
    KP2D_MONO_TASK --> KP2D_MONO_ID["ID_subject_XX"]

    %% 04 – Keypoints 3D mono‑subject
    ROOT --> KP3D_MONO["04_Keypoints3D_monosubject"]
    KP3D_MONO --> KP3D_MONO_METH["Methode2D_XX_Methode_3D_XX"]
    KP3D_MONO_METH --> KP3D_MONO_SUB["Sujet_XX"]
    KP3D_MONO_SUB --> KP3D_MONO_SESS["Session_XX"]
    KP3D_MONO_SESS --> KP3D_MONO_TASK["Tache_XX"]
    KP3D_MONO_TASK --> KP3D_MONO_ID["ID_subject_XX"]

    %% 05 – Ground‑truth 3D
    ROOT --> GT3D["05_Ground_truth_3D"]
    GT3D --> GT3D_METH["Methode2D_XX_Methode_3D_XX"]
    GT3D_METH --> GT3D_SUB["Sujet_XX"]
    GT3D_SUB --> GT3D_SESS["Session_XX"]
    GT3D_SESS --> GT3D_TASK["Tache_XX"]
    GT3D_TASK --> GT3D_ID["ID_subject_XX"]

    %% 06 – Ground‑truth 2D
    ROOT --> GT2D["06_Ground_truth_2D"]
    GT2D --> GT2D_METH["Methode_XX"]
    GT2D_METH --> GT2D_SUB["Sujet_XX"]
    GT2D_SUB --> GT2D_SESS["Session_XX"]
    GT2D_SESS --> GT2D_TASK["Tache_XX"]
    GT2D_TASK --> GT2D_ID["ID_subject_XX"]
````
