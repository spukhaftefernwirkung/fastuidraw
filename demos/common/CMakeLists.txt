set(SOURCES generic_command_line.cpp colorstop_command_line.cpp
	sdl_benchmark.cpp sdl_demo.cpp sdl_painter_demo.cpp PanZoomTracker.cpp
	ImageLoader.cpp read_colorstops.cpp read_path.cpp text_helper.cpp
	PainterWidget.cpp cycle_value.cpp random.cpp read_dash_pattern.cpp
	egl_gles_context.cpp)
add_library(demos-common STATIC ${SOURCES})
include(FindPkgConfig)
pkg_search_module(SDL2 REQUIRED sdl2)
target_include_directories(demos-common PUBLIC . PRIVATE ${SDL2_INCLUDE_DIRS})
target_link_libraries(demos-common PRIVATE FastUIDraw ${SDL2_LIBRARIES})
