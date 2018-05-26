cmake_minimum_required(VERSION 2.8.3)
project(roomba_500_series)

find_package(catkin REQUIRED COMPONENTS roscpp genmsg actionlib_msgs actionlib message_generation cereal_port tf)

add_message_files(
  FILES
  Battery.msg
  Brushes.msg
  Bumper.msg
  Buttons.msg
  Diagnostic.msg
  DigitLeds.msg		
  IRCharacter.msg
  Leds.msg
  Note.msg
  PlaySong.msg
  RoombaIR.msg
  RoombaSwitch.msg
  ScheduleLeds.msg
  Song.msg
  WheelDrop.msg
)

add_action_files(
  FILES
  GoDock.action
)

generate_messages(
  DEPENDENCIES
  std_msgs  
  actionlib_msgs
)

catkin_package(
   INCLUDE_DIRS include
   CATKIN_DEPENDS message_runtime actionlib_msgs
)


include_directories(include
  ${catkin_INCLUDE_DIRS}
)


add_library(irobot_oi src/iRobot/open_interface.cpp)
add_library(go_dock src/go_dock/go_dock.cpp)
add_dependencies(go_dock roomba_500_series_generate_messages_cpp)


add_executable(roomba560_node src/roomba560.cpp src/iRobot/open_interface.cpp src/go_dock/go_dock.cpp)
target_link_libraries(roomba560_node cereal_port ${catkin_LIBRARIES})
add_dependencies(roomba560_node roomba_500_series_generate_messages_cpp)

add_executable(roomba500_light_node src/roomba500_lightweight.cpp src/iRobot/open_interface.cpp)
target_link_libraries(roomba500_light_node cereal_port ${catkin_LIBRARIES})
add_dependencies(roomba500_light_node roomba_500_series_generate_messages_cpp)

add_executable(go_dock_server src/go_dock_server.cpp src/go_dock/go_dock.cpp)
target_link_libraries(go_dock_server ${catkin_LIBRARIES})
add_dependencies(go_dock_server roomba_500_series_generate_messages_cpp)

add_executable(go_dock_client src/go_dock_client.cpp)
target_link_libraries(go_dock_client ${catkin_LIBRARIES})
add_dependencies(go_dock_client roomba_500_series_generate_messages_cpp)