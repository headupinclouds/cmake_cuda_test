cmake_minimum_required(VERSION 3.8)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake/modules")

project(cuda_test)

# NOTE: This doesn't work with xcode generators
#enable_language("CUDA")

find_package(CUDA "9.0" REQUIRED)

option(CUDA_EXAMPLE_USE_CC_EXT "Use CC extensions for CUDA files" ON)

set(cpu_source_files cpu.cc)

if(CUDA_EXAMPLE_USE_CC_EXT)
  set(gpu_source_files gpu.cu.cc)
else()
  set(gpu_source_files gpu.cu)
endif()

set_source_files_properties(${gpu_source_files} PROPERTIES CUDA_SOURCE_PROPERTY_FORMAT OBJ)
cuda_add_library(cuda_lib STATIC ${gpu_source_files} ${cpu_source_files})
add_executable(cuda_exe main.cc)
target_link_libraries(cuda_exe cuda_lib)
target_compile_definitions(cuda_lib PUBLIC USE_CUDA=1)
