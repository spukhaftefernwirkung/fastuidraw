file(GLOB_RECURSE SHADER_RESOURCES
	RELATIVE ${CMAKE_CURRENT_LIST_DIR}
	shaders/*.resource_string)

add_shader_resources(image-test-shader-resources SHARER_RESOURCES_CPP ${SHADER_RESOURCES})

include(FindPkgConfig)
pkg_search_module(SDL2 REQUIRED sdl2)
pkg_search_module(SDL2IMAGE REQUIRED SDL2_image)

foreach(GL_FLAVOR ${GL_FLAVORS_TO_BUILD})
  add_executable(image-test-${GL_FLAVOR} main.cpp ${SHARER_RESOURCES_CPP})
  add_dependencies(image-test-${GL_FLAVOR} image-test-shader-resources)
  target_include_directories(image-test-${GL_FLAVOR} PRIVATE ${SDL2_INCLUDE_DIRS} ${SDL2IMAGE_INCLUDE_DIRS})
  target_link_libraries(image-test-${GL_FLAVOR} PRIVATE demos-common FastUIDraw FastUIDraw${GL_FLAVOR} ${SDL2_LIBRARIES} ${SDL2IMAGE_LIBRARIES})
endforeach()

