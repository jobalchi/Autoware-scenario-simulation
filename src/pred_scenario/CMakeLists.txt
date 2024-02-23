cmake_minimum_required(VERSION 3.0.2)
project(pred_scenario)

find_package(autoware_build_flags REQUIRED)

find_package(
  catkin REQUIRED COMPONENTS
    autoware_can_msgs
    geometry_msgs
    jsk_recognition_msgs
    libwaypoint_follower
    map_file
    op_planner
    op_ros_helpers
    op_simu  
    op_utility
    pcl_conversions
    pcl_ros
    roscpp
    sensor_msgs
    tf
    vector_map_msgs
)

catkin_package(
  INCLUDE_DIRS include
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

link_directories(lib)

add_executable(op_common_params src/op_common_params/op_common_params.cpp )
target_link_libraries(op_common_params ${catkin_LIBRARIES} ${PCL_LIBRARIES})

add_executable(
  op_trajectory_generator
  src/op_trajectory_generator/op_trajectory_generator.cpp
  src/op_trajectory_generator/op_trajectory_generator_core.cpp
)

target_link_libraries(op_trajectory_generator ${catkin_LIBRARIES} ${PCL_LIBRARIES})

add_executable(
  op_trajectory_evaluator
  src/op_trajectory_evaluator/op_trajectory_evaluator.cpp
  src/op_trajectory_evaluator/op_trajectory_evaluator_core.cpp
)

target_link_libraries(op_trajectory_evaluator ${catkin_LIBRARIES} ${PCL_LIBRARIES})

add_executable(
  op_behavior_selector
  src/op_behavior_selector/op_behavior_selector.cpp
  src/op_behavior_selector/op_behavior_selector_core.cpp
)

target_link_libraries(op_behavior_selector ${catkin_LIBRARIES} ${PCL_LIBRARIES})

add_executable(
  op_motion_predictor
  src/op_motion_predictor/op_motion_predictor.cpp
  src/op_motion_predictor/op_motion_predictor_core.cpp
)

target_link_libraries(op_motion_predictor ${catkin_LIBRARIES} ${PCL_LIBRARIES})

add_dependencies(
  op_common_params
  op_trajectory_generator
  op_trajectory_evaluator
  op_behavior_selector
  op_motion_predictor
  ${catkin_EXPORTED_TARGETS}
)

install(
  TARGETS
    op_common_params
    op_trajectory_generator
    op_trajectory_evaluator
    op_behavior_selector
    op_motion_predictor
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
        
install(
  DIRECTORY launch/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
)