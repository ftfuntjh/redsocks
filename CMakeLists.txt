cmake_minimum_required(VERSION 3.10)

project(redsocks C)

include(ExternalProject)

ExternalProject_Add(
        libevent
        GIT_REPOSITORY https://github.com/libevent/libevent.git
        GIT_TAG release-2.1.11-stable
        SOURCE_DIR ${CMAKE_SOURCE_DIR}/lib/libevent
        INSTALL_COMMAND ""
        BUILD_IN_SOURCE ON
)
set(levent_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/lib/libevent/include)
set(levent_LIBRARY_LOCATION ${CMAKE_SOURCE_DIR}/lib/libevent/lib/libevent.so)

add_executable(redsocks
        base.h base.c base64.h base64.c debug.c dnstc.h dnstc.c dnsu2t.h dnsu2t.c http-auth.h http-auth.c http-connect.c http-relay.c libc-compat.h libevent-compat.h
        list.h log.h log.c main.h main.c md5.h md5.c parser.h parser.c redsocks.h redsocks.c redudp.h redudp.c socks5.h socks5.c socks4.c utils.h utils.c config.h version.c
        version.c)
add_library(levent SHARED IMPORTED GLOBAL)
set_property(TARGET levent PROPERTY IMPORTED_LOCATION ${levent_LIBRARY_LOCATION})
set_target_properties(redsocks PROPERTIES COMPILE_FLAGS "-D_XOPEN_SOURCE=600 -D_DEFAULT_SOURCE -D_GNU_SOURCE -Wall")

add_dependencies(redsocks libevent)
target_include_directories(redsocks PRIVATE ${levent_INCLUDE_DIR})
target_link_libraries(redsocks levent)
