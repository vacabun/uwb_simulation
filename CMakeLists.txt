cmake_minimum_required(VERSION 3.5)
project(uwb_simulation)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()


find_package(ament_cmake REQUIRED)
find_package(ament_index_cpp REQUIRED)
find_package(rclcpp REQUIRED)
find_package(nav_msgs REQUIRED)
find_package(uwb_interfaces REQUIRED)
find_package(gz-transport12 QUIET REQUIRED OPTIONAL_COMPONENTS log)

add_executable(uwb_simulation_tag_to_anchor_node 
  src/tag_to_anchor.cpp
  src/tinyxml2.cpp)

target_include_directories(uwb_simulation_tag_to_anchor_node PUBLIC include)

set(GZ_TRANSPORT_VER ${gz-transport12_VERSION_MAJOR})
target_link_libraries(uwb_simulation_tag_to_anchor_node gz-transport${GZ_TRANSPORT_VER}::core)

ament_target_dependencies(uwb_simulation_tag_to_anchor_node rclcpp nav_msgs ament_index_cpp uwb_interfaces)

# install(TARGETS
# uwb_simulation_tag_to_anchor_node
#   DESTINATION lib/${PROJECT_NAME})

add_executable(uwb_simulation_tag_to_tag_node 
  src/tag_to_tag.cpp)

target_include_directories(uwb_simulation_tag_to_tag_node PUBLIC include)

set(GZ_TRANSPORT_VER ${gz-transport12_VERSION_MAJOR})
target_link_libraries(uwb_simulation_tag_to_tag_node gz-transport${GZ_TRANSPORT_VER}::core)

ament_target_dependencies(uwb_simulation_tag_to_tag_node rclcpp nav_msgs ament_index_cpp uwb_interfaces)

install(TARGETS
uwb_simulation_tag_to_anchor_node
uwb_simulation_tag_to_tag_node
  DESTINATION lib/${PROJECT_NAME})


install(DIRECTORY launch
  DESTINATION share/${PROJECT_NAME}
)
install(DIRECTORY config
  DESTINATION share/${PROJECT_NAME}
)

ament_package()
