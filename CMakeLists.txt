# set the project name
project(gkit2light)
cmake_minimum_required(VERSION 3.12)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED true)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")

# Deps
#find_package(OpenGL REQUIRED)
#find_package(SDL2 REQUIRED)
#find_package(SDL2_image REQUIRED)
#find_package(GLEW REQUIRED)

#Conan
include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

# gkit variables
if (APPLE)
  add_compile_definitions(GK_MACOS)
endif()


# gkit library
file(GLOB gkit_headers CONFIGURE_DEPENDS "${PROJECT_SOURCE_DIR}/src/gKit/*.h")
file(GLOB gkit_files CONFIGURE_DEPENDS "${PROJECT_SOURCE_DIR}/src/gKit/*.cpp")

add_library(gkit STATIC ${gkit_files} ${gkit_headers})
target_include_directories(gkit PUBLIC ${CONAN_LIBS})
target_link_libraries(gkit ${CONAN_LIBS})
target_compile_features(gkit PUBLIC cxx_std_11)
target_sources(gkit  PRIVATE  ${gkit_files}  PUBLIC  ${gkit_headers})

# IDEs should put the headers in a nice place
source_group(
  TREE "${PROJECT_SOURCE_DIR}/src/gKit/"
  PREFIX "Header Files"
  FILES ${gkit_headers})


# add the executable
add_executable(tuto7_camera tutos/tuto7_camera.cpp)
target_include_directories(tuto7_camera PRIVATE "${PROJECT_SOURCE_DIR}/src/gKit/")
target_link_libraries(tuto7_camera PRIVATE gkit)
