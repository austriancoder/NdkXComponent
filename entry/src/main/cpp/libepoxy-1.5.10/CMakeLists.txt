cmake_minimum_required(VERSION 3.16)
project(libepoxy VERSION 1.5.10 LANGUAGES C)

# Set build options
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)
# Include directories
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)
include_directories(${CMAKE_CURRENT_BINARY_DIR})

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src)

# add_subdirectory("src")
# add_subdirectory("include/epoxy")

# Collect sources
set(EPOXY_SOURCES
    "src/dispatch_common.c"
    "src/dispatch_egl.c"
    "src/egl_generated_dispatch.c"
    "src/gl_generated_dispatch.c"
)

if(PLATFORM_LINUX AND CMAKE_C_COMPILER_ID STREQUAL "GNU")
    set(COMMON_LDFLAGS "-Wl,-Bsymbolic-functions -Wl,-z,relro")
endif()


# Define dependencies
set(EPOXY_DEPS ${DL_LIBRARY})

if(PLATFORM_WINDOWS)
    list(APPEND EPOXY_DEPS ${OPENGL32_LIBRARY} ${GDI32_LIBRARY})
endif()


if(ENABLE_EGL AND EGL_FOUND)
    list(APPEND EPOXY_DEPS ${EGL_LIBRARIES})
endif()

# Build the epoxy library
add_library(epoxy SHARED ${EPOXY_SOURCES})
target_include_directories(epoxy PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_link_libraries(epoxy PRIVATE ${EPOXY_DEPS})
target_compile_options(epoxy PRIVATE ${VISIBILITY_FLAGS})
target_link_options(epoxy PRIVATE ${COMMON_LDFLAGS})
