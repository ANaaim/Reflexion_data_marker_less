```mermaid
graph TD
    ROOT["<strong>project_root</strong>"]

    %% –– Calibration
    ROOT --> CALIB["Calibration_video"]
    CALIB --> CALIB_ID01["ID01_calib"]
    CALIB_ID01 --> CALIB_ID01_EX["Extrinseque"]
    CALIB_ID01_EX --> CALIB_ID01_EX_C1["cam_01"]
    CALIB_ID01_EX --> CALIB_ID01_EX_C2["cam_02"]
    CALIB_ID01_EX --> CALIB_ID01_EX_CN["cam_n"]
    CALIB_ID01 --> CALIB_ID01_IN["Intrinseque"]
    CALIB_ID01_IN --> CALIB_ID01_IN_C1["cam_01"]
    CALIB_ID01_IN --> CALIB_ID01_IN_C2["cam_02"]
    CALIB_ID01_IN --> CALIB_ID01_IN_CN["cam_n"]
    CALIB --> CALIB_ID02["ID02_calib"]
    CALIB --> CALIB_IDN["IDn_calib"]

    %% –– Vidéos brutes
    ROOT --> DATA["Data_video"]
    DATA --> SUJ01["Sujet_01"]
    SUJ01 --> S01_SESS["Session"]
    S01_SESS --> S01_TASK["Tache"]
    S01_TASK --> S01_C1["cam_01"]
    S01_TASK --> S01_C2["cam_02"]
    S01_TASK --> S01_CN["cam_n"]

    DATA --> SUJ02["Sujet_02"]
    SUJ02 --> S02_SESS["Session"]
    S02_SESS --> S02_TASK["Tache"]
    S02_TASK --> S02_C1["cam_01"]
    S02_TASK --> S02_C2["cam_02"]
    S02_TASK --> S02_CN["cam_n"]

    DATA --> SUJN["Sujet_n"]
    SUJN --> SN_SESS["Session"]
    SN_SESS --> SN_TASK["Tache"]
    SN_TASK --> SN_C1["cam_01"]
    SN_TASK --> SN_C2["cam_02"]
    SN_TASK --> SN_CN["cam_n"]

    %% –– Résultats de détection
    ROOT --> KP2D["Keypoints2D_monosubject"]
    ROOT --> KPMS["Keypoints_multisubject_IDreseau"]
````
