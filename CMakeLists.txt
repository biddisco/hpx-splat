cmake_minimum_required(VERSION 3.1)
project(hpxsplat)

#--------------------------------------------------
# Find HPX
#--------------------------------------------------
set(CMAKE_FIND_PACKAGE_NO_PACKAGE_REGISTRY 1)

find_package(HPX)
include_directories(${HPX_INCLUDE_DIRS})
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

if(WIN32)
  add_definitions(/bigobj)
endif()

#--------------------------------------------------
# VTK
#--------------------------------------------------
option(HPXSPLAT_ENABLE_RENDERING "Enable rendering with VTK" OFF)

if(HPXSPLAT_ENABLE_RENDERING)
  find_package(ParaView)
  if (ParaView_FOUND)
    include(${ParaView_USE_FILE})
    add_definitions(-DHPXSPLAT_HAVE_VTK)
    set(VTK_LIBS
		  vtkTestingRendering
		  vtkImagingCore
		  vtkImagingSources
		  vtkIOImage
		  vtkFiltersCore
		  vtkInteractionStyle
		  vtkParallelMPI
		  ${VTK_LIBRARIES}
		  vtksys
		  ${MPI_LIBRARIES}
		  )
  endif()
endif()

#--------------------------------------------------
# HPX test
#--------------------------------------------------
ADD_EXECUTABLE(TestHPXSplatter 
  TestHPXSplatter.cxx
)

hpx_setup_target(TestHPXSplatter)

TARGET_LINK_LIBRARIES(TestHPXSplatter
  ${VTK_LIBS}
  iostreams_component
  ${Boost_LIBRARIES}
)

