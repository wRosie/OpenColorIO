# SPDX-License-Identifier: BSD-3-Clause
# Copyright Contributors to the OpenColorIO Project.

if(NOT OCIO_GL_ENABLED)
    message(WARNING "GL component missing. Skipping oglapphelpers.")
    return()
endif()

set(SOURCES
    glsl.cpp
    oglapp.cpp
)

set(INCLUDES
    glsl.h
    oglapp.h
)

add_library(oglapphelpers STATIC ${SOURCES})
set_target_properties(oglapphelpers PROPERTIES POSITION_INDEPENDENT_CODE ON)
set_target_properties(oglapphelpers PROPERTIES OUTPUT_NAME OpenColorIOoglapphelpers)

if(NOT BUILD_SHARED_LIBS)
    target_compile_definitions(oglapphelpers
        PRIVATE
            OpenColorIO_SKIP_IMPORTS
    )
endif()

if(APPLE)

    set(PLATFORM_COMPILE_FLAGS "${PLATFORM_COMPILE_FLAGS} -Wno-deprecated-declarations")

endif()

set_target_properties(oglapphelpers PROPERTIES 
    COMPILE_FLAGS "${PLATFORM_COMPILE_FLAGS}")

target_include_directories(oglapphelpers
    PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}
    PRIVATE
        ${OPENGL_INCLUDE_DIR}
        ${GLEW_INCLUDE_DIRS}
        ${GLUT_INCLUDE_DIR}
)

target_link_libraries(oglapphelpers
    PRIVATE
        OpenColorIO
        ${OPENGL_LIBRARIES}
        ${GLEW_LIBRARIES}
        ${GLUT_LIBRARIES}
)

install(TARGETS oglapphelpers 
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)

install(FILES ${INCLUDES}
    DESTINATION include/OpenColorIO/oglapphelpers
)