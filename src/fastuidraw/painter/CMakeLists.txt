set(SOURCES stroked_path.cpp filled_path.cpp
	painter_attribute_data.cpp
	painter_attribute_data_filler_path_fill.cpp
	painter_attribute_data_filler_glyphs.cpp
	painter_brush.cpp painter_stroke_params.cpp
	painter_dashed_stroke_params.cpp
	painter.cpp painter_enums.cpp
	painter_shader_data.cpp
	painter_clip_equations.cpp
	painter_item_matrix.cpp painter_header.cpp
	painter_shader.cpp painter_shader_set.cpp
	painter_dashed_stroke_shader_set.cpp painter_stroke_shader.cpp
	painter_glyph_shader.cpp painter_blend_shader_set.cpp
	painter_fill_shader.cpp
	packing/painter_backend.cpp packing/painter_draw.cpp packing/painter_packer.cpp
	)
add_library(painter OBJECT ${SOURCES})
target_compile_definitions(painter PUBLIC ${FASTUIDRAW_COMPILE_DEFINITIONS})
mark_private_sources_as_private(painter)
target_include_directories(painter PUBLIC ../../../inc)
set_target_properties(painter PROPERTIES POSITION_INDEPENDENT_CODE ON)
