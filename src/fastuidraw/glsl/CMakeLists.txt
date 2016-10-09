file(GLOB_RECURSE SHADER_RESOURCES
	RELATIVE ${CMAKE_CURRENT_LIST_DIR}
	shaders/*.resource_string)
set(SOURCES shader_source.cpp shader_code.cpp
	painter_item_shader_glsl.cpp painter_blend_shader_glsl.cpp
	painter_backend_glsl.cpp
	# Private sources:
	private/backend_shaders.cpp private/uber_shader_builder.cpp
	)
add_shader_resources(glsl-shader-resources SHARER_RESOURCES_CPP ${SHADER_RESOURCES})

add_library(glsl OBJECT ${SOURCES} ${SHARER_RESOURCES_CPP})
add_dependencies(glsl glsl-shader-resources)
target_compile_definitions(glsl PUBLIC ${FASTUIDRAW_COMPILE_DEFINITIONS})
mark_private_sources_as_private(glsl)
target_include_directories(glsl PUBLIC ../../../inc)
set_target_properties(glsl PROPERTIES POSITION_INDEPENDENT_CODE ON)
