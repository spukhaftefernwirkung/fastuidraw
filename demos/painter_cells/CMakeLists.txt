include(FindPkgConfig)
pkg_search_module(SDL2 REQUIRED sdl2)
pkg_search_module(SDL2IMAGE REQUIRED SDL2_image)

foreach(GL_FLAVOR ${GL_FLAVORS_TO_BUILD})
  add_executable(painter-cells-${GL_FLAVOR} main.cpp cell.cpp table.cpp cell_group.cpp)
  target_include_directories(painter-cells-${GL_FLAVOR} PRIVATE ${SDL2_INCLUDE_DIRS} ${SDL2IMAGE_INCLUDE_DIRS})
  target_link_libraries(painter-cells-${GL_FLAVOR} PRIVATE demos-common${GL_FLAVOR} FastUIDraw FastUIDraw${GL_FLAVOR} ${SDL2_LIBRARIES} ${SDL2IMAGE_LIBRARIES})
endforeach()

