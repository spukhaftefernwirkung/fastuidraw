add_subdirectory(../3rd_party/glu-tess glu-tess)

find_package(Boost COMPONENTS thread REQUIRED QUIET)
find_package(Freetype REQUIRED)

set(FASTUIDRAW_COMPILE_DEFINITIONS $<$<CONFIG:Debug>:FASTUIDRAW_VECTOR_BOUND_CHECK> $<$<CONFIG:Debug>:FASTUIDRAW_DEBUG>)
set(FASTUIDRAWGL_COMPILE_DEFINITIONS ${FASTUIDRAW_COMPILE_DEFINITIONS} $<$<CONFIG:Debug>:GL_DEBUG>)
set(FASTUIDRAWGLES_COMPILE_DEFINITIONS ${FASTUIDRAWGL_COMPILE_DEFINITIONS} FASTUIDRAW_GL_USE_GLES)

# Marks all sources matching private/* with visibility hidden.
function(mark_private_sources_as_private TARGET)
  if (("${CMAKE_SHARED_LIBRARY_SUFFIX}" STREQUAL ".so") OR ("${CMAKE_SHARED_LIBRARY_SUFFIX}" STREQUAL ".dylib"))
    get_target_property(SOURCES ${TARGET} SOURCES)
    foreach(SOURCE ${SOURCES})
      if("${SOURCE}" MATCHES ^private/)
        get_source_file_property(SOURCE_COMPILE_FLAGS ${SOURCE} COMPILE_FLAGS)
        if(SOURCE_COMPILE_FLAGS)
          set(SOURCE_COMPILE_FLAGS "${SOURCE_COMPILE_FLAGS} -fvisibility=hidden -fvisibility-inlines-hidden")
        else()
          set(SOURCE_COMPILE_FLAGS "-fvisibility=hidden -fvisibility-inlines-hidden")
        endif()
        set_source_files_properties(${SOURCE} PROPERTIES COMPILE_FLAGS ${SOURCE_COMPILE_FLAGS})
      endif()
    endforeach()
  endif()
endfunction()


# FastUIDraw library is made of:
#   - Set of own sources (current directory & private subdirectory).
#   - glu-tess
#   - Sources in util, text, painter and glsl subdirectory (and their private subdirectories if available).
set(SOURCES image.cpp colorstop.cpp colorstop_atlas.cpp path.cpp tessellated_path.cpp stroked_path.cpp filled_path.cpp
	# Private sources:
	private/interval_allocator.cpp private/path_util_private.cpp
	)
list(APPEND SOURCES $<TARGET_OBJECTS:glu-tess>)
set(PUBLIC_INCLUDE_DIRS $<TARGET_PROPERTY:glu-tess,INTERFACE_INCLUDE_DIRECTORIES>)
foreach(SUBDIR util text painter glsl)
  add_subdirectory(${SUBDIR})
  list(APPEND SOURCES $<TARGET_OBJECTS:${SUBDIR}>)
  list(APPEND PUBLIC_INCLUDE_DIRS $<TARGET_PROPERTY:${SUBDIR},INTERFACE_INCLUDE_DIRECTORIES>)
endforeach()

add_library(FastUIDraw SHARED ${SOURCES})
target_compile_definitions(FastUIDraw PUBLIC ${FASTUIDRAW_COMPILE_DEFINITIONS})
mark_private_sources_as_private(FastUIDraw)
target_include_directories(FastUIDraw PUBLIC ${PUBLIC_INCLUDE_DIRS})
target_link_libraries(FastUIDraw PUBLIC ${FREETYPE_LIBRARIES} Boost::boost PRIVATE Boost::thread)

# FastUIDrawGL(ES) library is made of sources in gl_backend subdirectory and uses the
# NGL(ES) library.
add_subdirectory(gl_backend)
foreach(GL_FLAVOR ${GL_FLAVORS_TO_BUILD})
  add_library(FastUIDraw${GL_FLAVOR} SHARED $<TARGET_OBJECTS:gl_backend${GL_FLAVOR}>)
  target_compile_definitions(FastUIDraw${GL_FLAVOR} PUBLIC ${FASTUIDRAW${GL_FLAVOR}_COMPILE_DEFINITIONS})    
  target_link_libraries(FastUIDraw${GL_FLAVOR} PUBLIC N${GL_FLAVOR} FastUIDraw)
endforeach()

