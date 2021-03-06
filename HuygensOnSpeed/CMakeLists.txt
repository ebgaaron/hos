
add_library(HuygensOnSpeed HuygensSimulator.cpp ObservationArea.cpp PaintTool.cpp PointSource.cpp)

add_library(HuygensCuComplex HuygensCuComplex.cpp)

# CUDA functionality
if (CALC_WITH STREQUAL CUDA OR DISP_WITH STREQUAL CUDA)
	add_library(CudaUtils CudaUtils.cpp)
endif ()

if (CALC_WITH STREQUAL CUDA)
   cuda_add_library(HuygensCuda HuygensOnGPU.cu)
   target_link_libraries(HuygensCuda HuygensCuComplex CudaUtils ${CUDA_CUDART_LIBRARY})
   target_link_libraries(HuygensOnSpeed HuygensCuda)
endif ()

if (DISP_WITH STREQUAL CUDA)
   cuda_add_library(DisplayCuda DisplayResponse.cu)
   target_link_libraries(DisplayCuda HuygensCuComplex CudaUtils ${CUDA_CUBLAS_LIBRARIES} ${CUDA_CUDART_LIBRARY})
   target_link_libraries(HuygensOnSpeed DisplayCuda)
endif ()


# CPU functionality
if (CALC_WITH STREQUAL CPU)
   add_library(HuygensCPU HuygensOnCPU.cpp)
   target_link_libraries(HuygensCPU HuygensCuComplex)
   target_link_libraries(HuygensOnSpeed HuygensCPU)
endif ()

if (DISP_WITH STREQUAL CPU)
   add_library(DisplayCPU DisplayResponseCPU.cpp)
   target_link_libraries(DisplayCPU HuygensCuComplex)
   if (UNIX)
      if (APPLE)

      else ()
         target_link_libraries(DisplayCPU GLU)
      endif ()
   endif()
   target_link_libraries(HuygensOnSpeed DisplayCPU)
endif ()

#add_library(HuygensOpenCL)
#target_link_libraries(HuygensOpenCL)

