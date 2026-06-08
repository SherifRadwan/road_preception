# 1. System Architecture Structure

The system is divided into four layers: Data Input, Perception Processing, Fusion & Logic, and Application/Output.


    [ Camera Stream ] --------> [ Computer Vision Pipeline ] 
                                    | (Signs, Lane Lines, Slots)
                                    v
                                [ Sensor Fusion Layer ] ----> [ Scene Understanding State Machine ] ----> [ User Interface / Control ]
                                    ^
                                    | (Object Positions, Speeds)
    [ Radar Stream  ] --------> [ Radar Processing Pipeline ]

## Layer 1: Data Input (Ingestion)

**Camera Module:**
Captures RGB video frames (or simulated frames from CARLA).

Radar Module: Captures target lists (range, azimuth angle, and relative velocity of objects).

## Layer 2: Perception Processing

### Vision Branch (AI):

- **Object Detection:** Identifies traffic signs (School zone, Parking).

- **Semantic Segmentation:** Detects road boundaries and parking slot lines.

### Radar Branch (Signal/Data Processing): 
- Filters out background noise (like static walls or ground clutter).

- Tracks moving or stationary obstacles ahead.

## Layer 3: Sensor Fusion & Scene Understanding (The Brain)

**Spatial Fusion:** Aligns the camera pixels with the radar's 3D coordinates so the system knows exactly where a detected sign or parking slot is in the real world.

**State Machine Logic:** Evaluates the environment.

**Example:** If a "School Zone" sign is detected and verified by distance, trigger STATE = SCHOOL_ZONE.

## Layer 4: Application & Output

- **Visualization:** A dashboard showing the camera feed with boxes around signs, overlaid with radar data, and a 2D map of the parking slots.

- **Control/Alerts:**Sending commands like "Slow down to 20 mph" or "Perpendicular parking slot found".

# 2. Directory Structure (Your Project Folder)

Create the following folder structure on your computer. This structure keeps your AI models, data scripts, and core logic completely separated and organized.


road_perception_project/
│
├── CMakeLists.txt               # Build configuration file 
├── config/
│   └── config.yaml              # Global settings (parsed using yaml-cpp)
│
├── data/
│   ├── sample_videos/
│   └── radar_logs/
│
├── include/                     # All header files (.hpp) defining classes & functions
│   ├── inputs/
│   │   ├── CameraStream.hpp
│   │   └── RadarStream.hpp
│   ├── perception/
│   │   ├── SignDetector.hpp
│   │   └── SlotSegmenter.hpp
│   ├── fusion/
│   │   └── SensorFusion.hpp
│   └── decision/
│       └── StateMachine.hpp
│
├── src/                         # All source implementation files (.cpp)
│   ├── inputs/
│   │   ├── CameraStream.cpp
│   │   └── RadarStream.cpp
│   ├── perception/
│   │   ├── SignDetector.cpp
│   │   └── SlotSegmenter.cpp
│   ├── fusion/
│   │   └── SensorFusion.cpp
│   └── decision/
│       └── StateMachine.cpp
│   └── main.cpp                 # Main entry point containing the real-time execution loop
│
└── models/
    └── yolov8_signs.engine      # Compiled TensorRT model optimized for C++ execution




# 3. Recommended from AI Blueprint to Build Code (Step-by-Step)




**Phase 1 (The Input):** Write src/inputs/camera_stream.py just to open your webcam or a test video file and display it on your screen using OpenCV.

**Phase 2 (The AI):** Write src/perception/sign_detector.py. Integrate a standard pre-trained YOLO model to detect generic objects (like cars or people) in your camera stream first. Once that works, swap it with a traffic sign dataset.

**Phase 3 (The Logic):** Write src/decision/state_machine.py. Make it so that if sign_detector.py sees a specific object, a text warning pops up on your video screen saying "SCHOOL ZONE DETECTED".

**Phase 4 (Advanced Geometry & Radar):** Once your camera reads the environment well, start adding the slot_segmenter.py geometry logic and the radar data streams.