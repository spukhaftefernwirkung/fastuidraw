set(GL_RAW_HEADER_FILES GL/glcorearb.h)
set(GLES_RAW_HEADER_FILES GLES3/gl3platform.h GLES3/gl3.h GLES3/gl31.h GLES3/gl32.h GLES2/gl2ext.h)

set(NGL_GL_CPP ngl_gl.cpp)
set(NGL_GL_HPP ${CMAKE_SOURCE_DIR}/inc/fastuidraw/gl_backend/ngl_gl.hpp)

set(NGL_GLES_CPP ngl_gles3.cpp)
set(NGL_GLES_HPP ${CMAKE_SOURCE_DIR}/inc/fastuidraw/gl_backend/ngl_gles3.hpp)

find_package(OpenGL REQUIRED)

foreach(GL_FLAVOR ${GL_FLAVORS_TO_BUILD})
  set(${GL_FLAVOR}_HEADER_FILES)
  foreach(HEADER_FILE ${${GL_FLAVOR}_RAW_HEADER_FILES})
    if (NOT EXISTS ${OPENGL_INCLUDE_DIR}/${HEADER_FILE})
      message(SEND_ERROR "${GL_FLAVOR} header file ${OPENGL_INCLUDE_DIR}/${HEADER_FILE} does not exist")
    else()
      list(APPEND ${GL_FLAVOR}_HEADER_FILES ${OPENGL_INCLUDE_DIR}/${HEADER_FILE})
    endif()
  endforeach()
  message(STATUS "${GL_FLAVOR} flavor requires headers ${${GL_FLAVOR}_HEADER_FILES}")

  set(EXTRACTOR_ARGS macro_prefix=FASTUIDRAWgl namespace=\"fastuidraw::gl_binding\" path=${OPENGL_INCLUDE_DIR} output_cpp=${NGL_${GL_FLAVOR}_CPP} output_hpp=${NGL_${GL_FLAVOR}_HPP} ${${GL_FLAVOR}_RAW_HEADER_FILES})

  add_custom_command(
    OUTPUT ${NGL_${GL_FLAVOR}_CPP} ${NGL_${GL_FLAVOR}_HPP}
    COMMAND filter ${${GL_FLAVOR}_HEADER_FILES} > ngl_${GL_FLAVOR}_filtered.txt
    COMMAND extractor ${EXTRACTOR_ARGS} < ngl_${GL_FLAVOR}_filtered.txt
    DEPENDS filter extractor
    )
  set_source_files_properties(${NGL_${GL_FLAVOR}_CPP} PROPERTIES GENERATED TRUE)
  add_custom_target(ngl_${GL_FLAVOR} DEPENDS ${NGL_${GL_FLAVOR}_CPP} ${NGL_${GL_FLAVOR}_HPP})

  # NGL/NGLES library:
  add_library(N${GL_FLAVOR} ../gl_binding.cpp ${NGL_${GL_FLAVOR}_CPP})
  target_compile_definitions(N${GL_FLAVOR} PUBLIC $<$<CONFIG:Debug>:GL_DEBUG>)
  add_dependencies(N${GL_FLAVOR} ngl_${GL_FLAVOR})
  target_include_directories(N${GL_FLAVOR} PUBLIC ../../../../inc)
endforeach()

