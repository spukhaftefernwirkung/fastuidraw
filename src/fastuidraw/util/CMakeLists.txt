set(SOURCES static_resource.cpp
	fastuidraw_memory.cpp util.cpp math.cpp blend_mode.cpp
	reference_count_mutex.cpp reference_count_atomic.cpp
	pixel_distance_math.cpp)
add_library(util OBJECT ${SOURCES})
target_compile_definitions(util PUBLIC ${FASTUIDRAW_COMPILE_DEFINITIONS})
mark_private_sources_as_private(util)
target_include_directories(util PUBLIC ../../../inc)
if(${BUILD_SHARED_LIBS})
  set_target_properties(util PROPERTIES POSITION_INDEPENDENT_CODE ON)
endif()

