cmake_minimum_required(VERSION 3.29.3 FATAL_ERROR)
project(Lane_recognition)


set(CMAKE_CXX_STANDARD 17)

set(CMAKE_PREFIX_PATH "E:/c++/libtorch/share/cmake/Torch")
set(CMAKE_PREFIX_PATH "E:/c++/opencv/build")

find_package(Torch REQUIRED)

find_package(OpenCV REQUIRED)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${TORCH_CXX_FLAGS}")
include_directories(${PROJECT_SOURCE_DIR}/include)
SET(CMAKE_INSTALL_PREFIX "E:/c++/libtorch/share/cmake/Torch")



set(CMAKE_CXX_COMPILER "cl")



add_executable(Lane_recognition src/main.cpp)
add_library(Lane_recognition)
target_link_libraries(Lane_recognition "${TORCH_LIBRARIES}")
target_include_directories(Lane_recognition  PRIVATE "${TORCH_INCLUDE_DIRS}")
include_directories(${OpenCV_INCLUDE_DIRS})
target_link_libraries(Lane_recognition ${OpenCV_LIBS})

if (MSVC)
  file(GLOB TORCH_DLLS "${TORCH_INSTALL_PREFIX}/lib/*.dll")
  add_custom_command(TARGET Lane_recognition
                     POST_BUILD
                     COMMAND ${CMAKE_COMMAND} -E copy_if_different
                     ${TORCH_DLLS}
                     $<TARGET_FILE_DIR:Lane_recognition>)
endif (MSVC)