cmake_minimum_required(VERSION 2.8)

if (NOT TOOLCHAIN)
  #set(TOOLCHAIN linux-gcc-4.6-arm-hard)
  #set(TOOLCHAIN linux-gcc-4.6-arm-softfp)
  #set(TOOLCHAIN linux-gcc-4.9-arm-hard)
  set(TOOLCHAIN linux-gcc-4.9-android-softfp)
  #set(TOOLCHAIN linux-gcc-4.9-x86_64)
endif ()

if (NOT CMAKE_TOOLCHAIN_FILE)
  set(CMAKE_TOOLCHAIN_FILE /data/public-space/toolchains/${TOOLCHAIN}-toolchain.cmake)
endif ()

project(nnpack C CXX)

set(SRC_FILES
  src/init.c
  src/convolution-output.c
  src/convolution-input-gradient.c
  src/convolution-kernel-gradient.c
  src/convolution-inference.c
  src/fully-connected-output.c
  src/fully-connected-inference.c
  src/pooling-output.c
  src/softmax-output.c
  src/relu-output.c
  src/relu-input-gradient.c
  
  src/psimd/2d-fourier-8x8.c
  src/psimd/2d-fourier-16x16.c
  src/psimd/softmax.c
  src/psimd/blas/shdotxf.c
  src/neon/relu.c
  src/neon/2d-winograd-8x8-3x3.c
  src/neon/2d-winograd-8x8-3x3-fp16.c
  src/neon/blas/conv1x1.c
  src/neon/blas/h4gemm.c
  src/neon/blas/s4gemm.c
  src/neon/blas/c4gemm.c
  src/neon/blas/s4c2gemm.c
  src/neon/blas/c4gemm-conjb.c
  src/neon/blas/s4c2gemm-conjb.c
  src/neon/blas/c4gemm-conjb-transc.c
  src/neon/blas/s4c2gemm-conjb-transc.c
  src/neon/blas/sgemm.c
  src/neon/blas/sdotxf.c

  
  src/threadpool-pthreads.c  

  ${ANDROID_NDK}/sources/android/cpufeatures/cpu-features.c
)


set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=gnu99 -mfpu=neon-fp16")
include_directories(include src ${ANDROID_NDK}/sources/android/cpufeatures)

set(LIB ${PROJECT_NAME})

add_library(${LIB} SHARED ${SRC_FILES})
target_link_libraries(${LIB} m)


