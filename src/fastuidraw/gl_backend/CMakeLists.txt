add_subdirectory(ngl_generator)
add_subdirectory(ngl)

file(GLOB_RECURSE SHADER_RESOURCES
	RELATIVE ${CMAKE_CURRENT_LIST_DIR}
	shaders/*.resource_string)
set(SOURCES gl_get.cpp opengl_trait.cpp gluniform_implement.cpp
	gl_program.cpp gl_context_properties.cpp
	image_gl.cpp colorstop_atlas_gl.cpp glyph_atlas_gl.cpp
	painter_backend_gl.cpp
	# Private sources:
	private/tex_buffer.cpp private/texture_gl.cpp private/texture_view.cpp
	)
add_shader_resources(gl_backend-shader-resources SHARER_RESOURCES_CPP ${SHADER_RESOURCES})

foreach(GL_FLAVOR ${GL_FLAVORS_TO_BUILD})
  add_library(gl_backend${GL_FLAVOR} OBJECT ${SOURCES} ${SHARER_RESOURCES_CPP})
  add_dependencies(gl_backend${GL_FLAVOR} gl_backend-shader-resources)
  # Building gl_blackend{GL,GLES} requires the generated ngl_{gl,gles}.hpp header file to be built.
  # And that happens in the ngl_{GL,GLES} target.
  add_dependencies(gl_backend${GL_FLAVOR} ngl_${GL_FLAVOR})
  target_compile_definitions(gl_backend${GL_FLAVOR} PUBLIC ${FASTUIDRAW${GL_FLAVOR}_COMPILE_DEFINES_DEBUG})
  mark_private_sources_as_private(gl_backend${GL_FLAVOR})
  target_include_directories(gl_backend${GL_FLAVOR} PUBLIC ../../../inc)
  set_target_properties(gl_backend${GL_FLAVOR} PROPERTIES POSITION_INDEPENDENT_CODE ON)
endforeach()
