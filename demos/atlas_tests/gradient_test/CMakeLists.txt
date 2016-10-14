file(GLOB_RECURSE SHADER_RESOURCES
	RELATIVE ${CMAKE_CURRENT_LIST_DIR}
	shaders/*.resource_string)

add_shader_resources(gradient-test-shader-resources SHARER_RESOURCES_CPP ${SHADER_RESOURCES})

include(FindPkgConfig)
pkg_search_module(SDL2 REQUIRED sdl2)
foreach(GL_FLAVOR ${GL_FLAVORS_TO_BUILD})
  add_executable(gradient-test-${GL_FLAVOR} main.cpp ${SHARER_RESOURCES_CPP})
  add_dependencies(gradient-test-${GL_FLAVOR} gradient-test-shader-resources)
  target_include_directories(gradient-test-${GL_FLAVOR} PRIVATE ${SDL2_INCLUDE_DIRS})
  target_link_libraries(gradient-test-${GL_FLAVOR} PRIVATE demos-common${GL_FLAVOR} FastUIDraw FastUIDraw${GL_FLAVOR} ${SDL2_LIBRARIES})
endforeach()

