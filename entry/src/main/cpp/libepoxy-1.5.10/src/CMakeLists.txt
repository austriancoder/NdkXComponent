set(GEN_DISPATCH_PY ${CMAKE_CURRENT_SOURCE_DIR}/gen_dispatch.py)

set(GENERATED_SOURCES "")

# Function to create custom commands for source generation
function(generate_dispatch output_file registry additional_sources)
    set(GENERATED_FILE ${CMAKE_CURRENT_SOURCE_DIR}/${output_file})

    add_custom_command(
        OUTPUT ${GENERATED_FILE}
        COMMAND ${PYTHON_EXECUTABLE} ${GEN_DISPATCH_PY} --source --no-header --outputdir=${CMAKE_CURRENT_SOURCE_DIR} ${registry}
        DEPENDS ${registry} ${GEN_DISPATCH_PY}
        COMMENT "Generating ${output_file}"
        VERBATIM
    )

    list(APPEND GENERATED_SOURCES ${GENERATED_FILE} ${additional_sources})
    set(GENERATED_SOURCES ${GENERATED_SOURCES} PARENT_SCOPE)
endfunction()

# Generate sources
generate_dispatch("gl_generated_dispatch.c" ${CMAKE_CURRENT_SOURCE_DIR}/registry/gl.xml "+dispatch_common.c;dispatch_common.h")
generate_dispatch("egl_generated_dispatch.c" ${CMAKE_CURRENT_SOURCE_DIR}/registry/egl.xml "dispatch_egl.c")

MESSAGE(aa ${GENERATED_SOURCES})