cmake_minimum_required(VERSION 3.10)
project(RoadPerceptionSystem VERSION 1.0)

# Set C++ standard to C++17 or C++20 for modern features
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Find OpenCV package
find_package(OpenCV REQUIRED)

# Include directories
include_directories(include)

# Define the executable and its source files
add_executable(RoadPerceptionSystem
    src/main.cpp
    src/CameraStream.cpp
    src/SignDetector.cpp
    src/StateMachine.cpp
)

# Link OpenCV libraries to your project
target_link_libraries(RoadPerceptionSystem ${OpenCV_LIBS})