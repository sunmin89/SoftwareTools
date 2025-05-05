cmake_minimum_required(VERSION 3.10)
project(SoftwareTools C)

set(CMAKE_C_STANDARD 99)

# create libst.a 
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/lib/build)

# define libst.a
add_library(st STATIC
    lib/src/args.c
    lib/src/io.c
    lib/src/line.c
    lib/src/error.c
    lib/src/str.c
)

target_include_directories(st PUBLIC lib/include)

# install libst.a and to CMAKE_INSTALL_PREFIX/lib/
install(TARGETS st
    ARCHIVE DESTINATION lib
)
# install header files to CMAKE_INSTALL_PREFIX/include/
install(DIRECTORY lib/include/
    DESTINATION include
)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/tools/build)
file(GLOB TOOL_SOURCES tools/src/*.c)
foreach(tool_src ${TOOL_SOURCES})
    get_filename_component(tool_name ${tool_src} NAME_WE)
    add_executable(${tool_name} ${tool_src})
    target_link_libraries(${tool_name} PRIVATE st)
    
    target_include_directories(${tool_name} PRIVATE
        tools/include
        lib/include
    )

     # install binary files to CMAKE_INSTALL_PREFIX/bin
     install(TARGETS ${tool_name}
     RUNTIME DESTINATION bin
 )
endforeach()
