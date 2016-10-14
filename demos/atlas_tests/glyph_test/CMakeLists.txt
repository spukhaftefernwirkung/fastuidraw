file(GLOB_RECURSE SHADER_RESOURCES
	RELATIVE ${CMAKE_CURRENT_LIST_DIR}
	shaders/*.resource_string)

add_shader_resources(glyph-test-shader-resources SHARER_RESOURCES_CPP ${SHADER_RESOURCES})

include(FindPkgConfig)
pkg_search_module(SDL2 REQUIRED sdl2)

foreach(GL_FLAVOR ${GL_FLAVORS_TO_BUILD})
  add_executable(glyph-test-${GL_FLAVOR} main.cpp ${SHARER_RESOURCES_CPP})
  add_dependencies(glyph-test-${GL_FLAVOR} glyph-test-shader-resources)
  target_include_directories(glyph-test-${GL_FLAVOR} PRIVATE ${SDL2_INCLUDE_DIRS})
  target_link_libraries(glyph-test-${GL_FLAVOR} PRIVATE demos-common${GL_FLAVOR} FastUIDraw FastUIDraw${GL_FLAVOR} ${SDL2_LIBRARIES})
endforeach()

