include(FindPkgConfig)
pkg_search_module(SDL2 REQUIRED sdl2)

foreach(GL_FLAVOR ${GL_FLAVORS_TO_BUILD})
  add_executable(painter-glyph-test-${GL_FLAVOR} main.cpp)
  target_include_directories(painter-glyph-test-${GL_FLAVOR} PRIVATE ${SDL2_INCLUDE_DIRS})
  target_link_libraries(painter-glyph-test-${GL_FLAVOR} PRIVATE demos-common${GL_FLAVOR} FastUIDraw FastUIDraw${GL_FLAVOR} ${SDL2_LIBRARIES})
endforeach()

