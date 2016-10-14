set(SOURCES generic_command_line.cpp colorstop_command_line.cpp
	sdl_benchmark.cpp sdl_demo.cpp sdl_painter_demo.cpp PanZoomTracker.cpp
	ImageLoader.cpp read_colorstops.cpp read_path.cpp text_helper.cpp
	PainterWidget.cpp cycle_value.cpp random.cpp read_dash_pattern.cpp
	egl_gles_context.cpp)
include(FindPkgConfig)
pkg_search_module(SDL2 REQUIRED sdl2)
foreach(GL_FLAVOR ${GL_FLAVORS_TO_BUILD})
  if (${GL_FLAVOR} STREQUAL "GLES")
    set(EXTRA_LIBRARIES EGL GLESv2)
  else()
    set(EXTRA_LIBRARIES)
  endif()
  add_library(demos-common${GL_FLAVOR} STATIC ${SOURCES})
  target_include_directories(demos-common${GL_FLAVOR} PUBLIC . PRIVATE ${SDL2_INCLUDE_DIRS})
  target_link_libraries(demos-common${GL_FLAVOR} PRIVATE FastUIDraw FastUIDraw${GL_FLAVOR} ${SDL2_LIBRARIES} ${EXTRA_LIBRARIES})
endforeach()
