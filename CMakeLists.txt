cmake_minimum_required(VERSION 2.8)

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
)


set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=gnu99 -mfpu=neon-fp16")

include_directories(include src)

if (ANDROID)
  include_directories(${ANDROID_NDK}/sources/android/cpufeatures)
  list(APPEND SRC_FILES ${ANDROID_NDK}/sources/android/cpufeatures/cpu-features.c)
endif ()

set(LIB ${PROJECT_NAME})

add_library(${LIB} ${SRC_FILES})
target_link_libraries(${LIB} m)

set(CMAKE_INSTALL_PREFIX ${PROJECT_BINARY_DIR}/install/${PROJECT_NAME})
install(TARGETS ${LIB} DESTINATION lib)
install(FILES include/nnpack.h include/pthreadpool.h DESTINATION include)
install(TARGETS ${LIB} DESTINATION lib)
