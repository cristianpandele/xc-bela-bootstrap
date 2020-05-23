add_executable(sinetone
    ${CMAKE_SOURCE_DIR}/Bela/core/default_main.cpp
    render.cpp
)

target_link_libraries(sinetone PRIVATE
    ${CMAKE_SYSROOT}/root/Bela/lib/libbelafull.a
)

add_custom_command(
    TARGET sinetone
    POST_BUILD
    COMMAND scp ${CMAKE_BINARY_DIR}/bin/sinetone root@$ENV{BBB_HOSTNAME}:~
)