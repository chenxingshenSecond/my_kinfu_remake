cmake_minimum_required(VERSION 2.8.8)

# ---[ Configurations types
set(CMAKE_CONFIGURATION_TYPES "DEBUG" CACHE STRING "Possible configurations" FORCE)
if (DEFINED CMAKE_BUILD_TYPE)
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS ${CMAKE_CONFIGURATION_TYPES})
endif()

# ---[ Solution name
project(kfusion C CXX)

# ---[ utility
list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/Modules/")
include(cmake/Utils.cmake)
include(cmake/Targets.cmake)

# ---[ find dependencies
find_package(CUDA 8.0 REQUIRED)
find_package(OpenNI REQUIRED)
find_package(OpenCV 2.4.9 REQUIRED COMPONENTS core viz highgui)
include_directories(${OpenCV_INCLUDE_DIRS} ${CUDA_INCLUDE_DIRS} ${OPENNI_INCLUDE_DIR})

#set (CUDA_NVCC_FLAGS ${CUDA_NVCC_FLAGS}  "-G;-g")

# ---[ misc settings
if(USE_PROJECT_FOLDERS)
  set_property(GLOBAL PROPERTY USE_FOLDERS ON)
  set_property(GLOBAL PROPERTY PREDEFINED_TARGETS_FOLDER "CMakeTargets")
endif()

# ---[ cuda settings
set(HAVE_CUDA 1)
#list(APPEND CUDA_NVCC_FLAGS "-arch;compute_20")
#list(APPEND CUDA_NVCC_FLAGS "-gencode;arch=compute_20,code=sm_20;-gencode;arch=compute_20,code=sm_21;-gencode;arch=compute_30,code=sm_30;-gencode;arch=compute_35,code=sm_35;-gencode;arch=compute_50,code=sm_50;-gencode;arch=compute_60,code=sm_60;-gencode;arch=compute_62,code=sm_62;")
list(APPEND CUDA_NVCC_FLAGS "-gencode;arch=compute_61,code=sm_61")

if(UNIX OR APPLE)
  list(APPEND CUDA_NVCC_FLAGS "-Xcompiler;-fPIC;")
endif()

warnings_disable(CMAKE_CXX_FLAGS /wd4985)

add_subdirectory(kfusion)
add_subdirectory(apps) 
