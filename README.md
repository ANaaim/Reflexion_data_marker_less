# Data Standardisation for Markerless Acquisition

## Scope of Standardisation

We need to determine the appropriate scope for our standardisation efforts:

1. **For General Markerless Data:**
   - Focus on standardizing trials with emphasis on minimal required metadata
   - This approach could lead to code that enables users to generate sharable data

2. **For Comparative Studies of Different Methods:**
   - Focus on standardizing by data type (videos, calibration data, etc.)
   - This facilitates easier comparison between different approaches

**The purpose of standardisation must be clearly defined as it will guide our approach.**

In either case, three key elements need standardisation:
- File formats for each data type (.avi, .hdf5, .toml, etc.)
- Minimum set of required metadata
- Overall data structure organization

## Data Structure

### Global Folder Structure

The hierarchical structure (subject_XX, session_XX, trial_XX) is not mandatory but provides organizational benefits. The critical requirement is that each final folder should be self-contained and processable independently.

Information about subjects, sessions, and trials is stored as metadata in the `metadata_dataset.h5` file through:
- **folder_depth**: Indicates the number of folders in the hierarchy path (e.g., a depth of 3 means first folder is subject, second is session, third is trial)
- Nested dictionaries storing the relevant information for easy access

> Note: While "depth" might be sufficient as terminology, "folder_depth" helps avoid confusion with RGB-D camera data.

In all organizational schemes, the following structure can be used in any order as long as the **folder_depth** parameter is properly set in the metadata:

```
├── Type_data_folder
│   └── subject_XX
│       └── session_XX
│           └── trial_XX
│               └── data_files
```

For simplicity, in subsequent diagrams this hierarchical structure will be referred to as **folder_organisation**.

### Organizational Approaches

#### 1. Organization by Data Type

This approach treats each folder as a self-contained unit of data (calibration, video, keypoints) defined by corresponding metadata files.

**Advantages:**
- Each leaf folder is self-contained and independently processable
- Sharing specific types of data is straightforward (e.g., share only 2D data)

**Structure example:**
```
├── Metadata_data_set.toml
├── 00_calibration_data
│   └── IDXX_calib
│       ├── calib_matrix.toml
│       ├── metadata_video_calibration.toml
│       ├── extrinsics
│       │   └── cam_XX
│       │       └── cam_XX.avi
│       └── intrinsics
│           └── cam_XX
│               └── cam_XX.avi
├── 01_calibration_matrix
│   └── folder_organisation
│       └── calib.toml
├── 02_data_video
│   └── folder_organisation
│       ├── calib_mat_from_IDXX.toml (optional)
│       ├── metadata_video.toml
│       └── video
│           └── cam_XX
│               └── cam_XX.avi or cam_XX.jpg
├── 03_keypoints_2D_multisubject
│   ├── metadata_method_origin_data
│   └── folder_organisation
│       └── data_multisubject.hdf5
├── 04_keypoints_2D_monosubject
│   ├── metadata_method_origin_data
│   └── folder_organisation
│       └── ID_subject_XX
│           └── data_monosubject.hdf5
└── 05_keypoints_3D_monosubject
    ├── metadata_method_origin_data
    └── folder_organisation
        └── ID_subject_XX
            ├── data_mono_person.c3d
            └── data_mono_person.hdf5
```

#### 2. Organization by Trial

This approach treats each trial as a complete unit containing all necessary data and metadata.

**Advantages:**
- Each trial folder is holistic and independently processable
- Allows flexible folder structure with session/subject/trial information in metadata

**Challenges:**
- Calibration data may need to be repeated or linked across trial folders
- Less efficient for method comparison (multiple method folders within each trial)

**Structure example:**
```
├── Metadata_data_set.toml
└── folder_organisation
    ├── 00_calibration_video
    │   └── IDXX_calib
    │       ├── calib_matrix.toml
    │       ├── metadata_video_calibration.toml
    │       ├── extrinsics
    │       │   └── cam_XX
    │       │       └── cam_XX.avi
    │       └── intrinsics
    │           └── cam_XX
    │               └── cam_XX.avi
    ├── 01_calibration_data
    │   └── calib_matrix.toml
    ├── 02_data_video
    │   ├── metadata_video.toml
    │   ├── calib_mat_from_IDXX.toml (optional)
    │   └── video
    │       └── cam_XX
    │           └── cam_XX.avi or cam_XX.jpg
    ├── 03_keypoints_2D_multisubject
    │   ├── metadata_method_origin_data
    │   └── data_multisubject.hdf5
    ├── 04_keypoints_2D_monosubject
    │   ├── metadata_method_origin_data
    │   └── ID_subject_XX
    │       └── data_monosubject.hdf5
    └── 05_keypoints_3D_monosubject
        ├── metadata_method_origin_data
        └── ID_subject_XX
            ├── data_mono_person.c3d
            └── data_mono_person.hdf5
```

In this organization, each folder contains a single TRIAL with all necessary metadata, mirroring the actual acquisition process.

## Global Considerations

### Philosophy of Data Organization

While this approach may appear to duplicate data, it serves important purposes:
- Each leaf folder can be processed independently
- Specific data components can be easily shared without reorganizing content
- It supports a modular processing workflow

To reduce duplication, we can consider adopting a "data type" perspective:
- Calibration video data
- Calibration matrices
- Video data
- 2D keypoints (multi-subject)
- 2D keypoints (mono-subject)
- 3D keypoints (mono-subject)

This object-oriented approach makes the code more modular—you can process specific data types independently (e.g., generate 2D keypoints from video without requiring calibration).

### File Formats

- **Metadata:** TOML format is preferred for its readability, ease of parsing, and generation
- **Array Data:** HDF5 format is recommended for its wide scientific use, efficient storage of large datasets, and compression capabilities
- When using HDF5 for data, metadata should also be stored within the HDF5 file to ensure data remains self-contained and shareable

## Specific Data Types

### 00_calibration_video

#### Video Format
The optimal video format requires further discussion. Generally, lossless compression with highest quality is preferred.

Camera-specific folders (e.g., cam_XX/) are maintained to accommodate image exports and match software requirements like Theia.

**Recommended codecs and formats** (pending validation from computer vision experts):

| Codec          | Format                                                              |
| -------------- | ------------------------------------------------------------------- |
| FFV1           | .mkv / .avi                                                         |
| H.264 lossless | .mp4 / .mkv (Use -qp 0 with libx264, but not always byte-identical) |

#### Metadata
See `description_metadata.md` file for details about the `metadata_video.toml` file.

### 01_calibration_data

#### Calibration Matrix Format
Currently using LBMC's format (P2S compatibility). Other formats could be supported with converters.

OpenCV format appears to be the most common standard for camera intrinsics and extrinsics matrices:

```toml
[cam_01]
name = "M11139"
size = [ 1920.0, 1080.0]
matrix = [ [ 1237.0284339307132, 0.0, 951.8049061116914], [ 0.0, 1239.2592485704645, 528.5344016190471], [ 0.0, 0.0, 1.0]]
distortions = [ -0.13060079196920754, 0.13400864981470154, 0.00018961778799407008, -0.0010206183805026338]
rotation = [ 0.9618561526427922, 1.5294483485830126, 0.5242193500626191]
translation = [ -0.6270525677142821, -0.486046176284735, 1.664813522720072]
fisheye = false

# ...additional cameras...

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
```

### 02_data_video
Similar organization to `00_calibration_data`.

**Open questions:**
- Should videos be provided in both raw and undistorted formats?
- Should video orientation be standardized?

The `calib_mat.toml` file contains camera calibration information (intrinsics and extrinsics).

### 03_keypoints_2D_multisubject

#### data_multi_person.hdf5
Structure for bounding boxes (format: [x1, y1, x2, y2]) and keypoints (format: [x, y, confidence]):

```
data_multi_person.hdf5
    |
    +--- cam_01
    |       |--- frame_01
    |       |       |---Id_XX
    |       |       |     |---bbox ==> 4x1 int array [x1,y1,x2,y2] if no bbox detected [NaN, NaN NaN, NaN]
    |       |       |     \---keypoints ==> 3xNb_Keypoint float [x,y,confidence] if no keypoint detected [NaN, NaN,NaN]
    |       |       \--- ...
    |       \--- frame_XX
    |
    +--- cam_XX
    |       |--- frame_01
    |       |       |---Id_XX
    |       |       |     |---bbox ==> 4x1 int array [x1,y1,x2,y2] if no bbox detected [NaN, NaN, NaN, NaN]
    |       |       |     \---keypoints ==> 3xNb_Keypoint float [x,y,confidence] if no keypoint detected [NaN, NaN,NaN]
    |       |       \--- ...
    |       \--- frame_XX
    |
    \--- metadata
            |---dictionary name point to their corresponding indices in the keypoints array
            \---TODO list metadata
```

### 04_keypoints_2D_monosubject

Same format as multi-subject data but structured for a single subject:

```
data_mono_person.hdf5
    +--- cam_01
    |       |--- bbox ==> 4xnb_frame int array [x1,y1,x2,y2,n] if no bbox detected [NaN, NaN NaN, NaN,i]
    |       \--- keypoints ==> 3xNb_Keypointxnb_frame float [x,y,confidence,n] if no keypoint detected [NaN, NaN,NaN,i]
    |
    +--- cam_XX
    |       |--- bbox ==> 4xnb_frame int array [x1,y1,x2,y2,n] if no bbox detected [NaN, NaN NaN, NaN,i]
    |       \--- keypoints ==> 3xNb_Keypointxnb_frame float [x,y,confidence,n] if no keypoint detected [NaN, NaN,NaN,i]
    |
    \---metadata
            |---Calib_matrix
            |---dictionary name point to their corresponding indices in the keypoints array
            \---TODO list metadata (see description_metadata.md)
```

### 05_keypoints_3D_monosubject

The c3d format is common in biomechanics while HDF5 is more general. A solution could be to use HDF5 as the primary format and provide converters to c3d using tools like ezc3d.

```
data_mono_person.hdf5
    |
    +---points_3D ==> 3xNb_Keypointxnb_frame float [x,y,z]
    | 
    \---metadata
            |---dictionary name point to their corresponding indices in the keypoints array
            \---TODO list metadata (see description_metadata.md)
```

### Ground Truth Data

Ground truth 2D and 3D data should follow the same formats as the keypoints data.

An alternative would be to integrate ground truth as part of the existing keypoints data structure with method names "ground_truth_2D" and "ground_truth_3D". This would consolidate all data in the same place for easier processing.

Both approaches have merit and should be discussed further.