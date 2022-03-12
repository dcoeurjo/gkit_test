# set the project name
project(gkit2light)
cmake_minimum_required(VERSION 3.11)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED true)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")

find_package(OpenGL REQUIRED)
find_package(SDL2 REQUIRED)
find_package(SDL2_image REQUIRED)
find_package(GLEW REQUIRED)

if (APPLE)
  add_compile_definitions(GK_MACOS)
endif()


# gkit library
file(GLOB gkit_files src/gKit/*.cpp)
add_library(gkit STATIC ${gkit_files})
target_include_directories(gkit PUBLIC ${SDL2_INCLUDE_DIR} ${SDL2_IMAGE_INCLUDE_DIRS} ${GLEW_INCLUDE_DIRS})
target_include_directories(gkit PUBLIC ${SDL2_INCLUDE_DIR})
target_link_libraries(gkit OpenGL::GL ${GLEW_LIBRARIES} ${SDL2_LIBRARY} ${SDL2_IMAGE_LIBRARIES})
target_compile_features(gkit PUBLIC cxx_std_11)



# add the executable
add_executable(tuto7_camera "tutos/tuto7_camera.cpp" ${gkit_files})
target_link_libraries(tuto7_camera PUBLIC gkit)
set_target_properties(tuto7_camera PROPERTIES RUNTIME_OUTPUT_DIRECTORY bin)
