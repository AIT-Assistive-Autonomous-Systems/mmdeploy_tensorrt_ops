cmake_minimum_required(VERSION 3.17)
project(mmdeploy_tensorrt_ops LANGUAGES CUDA CXX)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wpedantic")  
set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -Xcompiler=-Wall") 

find_package(ament_cmake_auto REQUIRED)
find_package(CUDAToolkit)

ament_auto_add_library(${PROJECT_NAME} SHARED
    plugins/tensorrt/modulated_deform_conv/trt_modulated_deform_conv.cpp
    plugins/tensorrt/modulated_deform_conv/trt_modulated_deform_conv_kernel.cu
    plugins/tensorrt/grid_sampler/trt_grid_sampler.cpp
    plugins/tensorrt/common_impl/trt_cuda_helper.cu
    plugins/tensorrt/grid_sampler/trt_grid_sampler_kernel.cu)

target_include_directories(${PROJECT_NAME} PRIVATE
    plugins/common/
    plugins/tensorrt/modulated_deform_conv
    plugins/tensorrt/grid_sampler
    plugins/tensorrt/common)

set_target_properties(${PROJECT_NAME} PROPERTIES
    CXX_STANDARD "17"
    CXX_STANDARD_REQUIRED "YES"
    CXX_EXTENSIONS "NO")

target_link_libraries(${PROJECT_NAME}
    CUDA::cublas
    nvinfer
    nvinfer_plugin
    nvparsers)

ament_auto_package()
