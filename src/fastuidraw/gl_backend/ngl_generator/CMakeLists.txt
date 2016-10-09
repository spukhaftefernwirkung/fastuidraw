# 'filter' target:
add_executable(filter filter.cpp)
# We don't want to inherit CMAKE_RUNTIME_OUTPUT_DIRECTORY, this is just a build tool.
set_target_properties(filter PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})

# 'extractor' target:
find_package(FLEX)
flex_target(gl_flex gl_flex.fl.cpp ${CMAKE_CURRENT_BINARY_DIR}/gl_flex.cpp)
add_executable(extractor HeaderCreator.cpp ${FLEX_gl_flex_OUTPUTS})
target_include_directories(extractor PRIVATE ${CMAKE_CURRENT_LIST_DIR})
# We don't want to inherit CMAKE_RUNTIME_OUTPUT_DIRECTORY, this is just a build tool.
set_target_properties(extractor PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})

