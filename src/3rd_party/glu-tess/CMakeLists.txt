add_library(glu-tess OBJECT dict.cpp geom.cpp memalloc.cpp mesh.cpp priorityq.cpp render.cpp sweep.cpp tess.cpp tessmono.cpp)
target_include_directories(glu-tess PUBLIC ../../../inc)
if(${BUILD_SHARED_LIBS})
	set_target_properties(glu-tess PROPERTIES POSITION_INDEPENDENT_CODE ON)
endif()

