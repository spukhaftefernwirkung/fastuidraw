find_package(Boost COMPONENTS thread REQUIRED QUIET)
find_package(Freetype REQUIRED)
set(SOURCES glyph_atlas.cpp
	glyph_render_data.cpp
	glyph_render_data_curve_pair.cpp
	glyph_render_data_distance_field.cpp
	glyph_render_data_coverage.cpp
	glyph_cache.cpp glyph_selector.cpp
	freetype_font.cpp freetype_lib.cpp
	font_properties.cpp
	# Private sources:
	private/rect_atlas.cpp private/freetype_util.cpp private/freetype_curvepair_util.cpp
	)
add_library(text OBJECT ${SOURCES})
target_compile_definitions(text PUBLIC ${FASTUIDRAW_COMPILE_DEFINITIONS})
target_include_directories(text PUBLIC ../../../inc ${FREETYPE_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS})
mark_private_sources_as_private(text)
if(${BUILD_SHARED_LIBS})
  set_target_properties(text PROPERTIES POSITION_INDEPENDENT_CODE ON)
endif()

