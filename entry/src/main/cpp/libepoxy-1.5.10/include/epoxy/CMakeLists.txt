set(GEN_DISPATCH_PY ${CMAKE_CURRENT_SOURCE_DIR}/../../src/gen_dispatch.py)

# List of generated headers
set(GENERATED_HEADERS "")

# Function to generate headers using gen_dispatch.py
function(generate_header output_header generated_header registry)
    set(GENERATED_FILE ${CMAKE_CURRENT_BINARY_DIR}/${generated_header})

    add_custom_command(
        OUTPUT ${GENERATED_FILE}
        COMMAND ${PYTHON_EXECUTABLE} ${GEN_DISPATCH_PY}
            --header --no-source --outputdir=${CMAKE_CURRENT_SOURCE_DIR} ${registry}
        DEPENDS ${registry} ${GEN_DISPATCH_PY}
        COMMENT "Generating ${generated_header}"
        VERBATIM
    )

    list(APPEND GENERATED_HEADERS ${GENERATED_FILE})
    set(GENERATED_HEADERS ${GENERATED_HEADERS} PARENT_SCOPE)
endfunction()

# Always generate GL headers
generate_header("gl.h" "gl_generated.h" ${CMAKE_CURRENT_SOURCE_DIR}../../registry/gl.xml)
generate_header("egl.h" "egl_generated.h" ${CMAKE_CURRENT_SOURCE_DIR}../../registry/egl.xml)
