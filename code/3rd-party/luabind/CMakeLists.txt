cmake_minimum_required(VERSION 3.8)

project(luabind)

file(GLOB HEADERS luabind/include/luabind/*.h luabind/include/luabind/*.hpp)
file(GLOB HEADERS_DETAIL luabind/include/luabind/detail/*.hpp)
file(GLOB SRC luabind/src/*.cpp)
file(GLOB ROOT_SRC *.h *.cpp)

source_group("include" FILES ${HEADERS})
source_group("include\\detail" FILES ${HEADERS_DETAIL})
source_group("src" FILES ${SRC})

link_directories(${CMAKE_ARCHIVE_OUTPUT_DIRECTORY})

add_definitions(-DLUABIND_BUILDING -D_SECURE_SCL=0)

add_compile_options(/fp:fast /wd4100 "/Yupch.h")
set_source_files_properties(pch.cpp PROPERTIES COMPILE_FLAGS /Yc)

add_library(luabind SHARED ${HEADERS} ${HEADERS_DETAIL} ${SRC} ${ROOT_SRC})
target_link_libraries(luabind luajit)
target_include_directories(luabind PUBLIC luabind/include
                                   PRIVATE ${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_SOURCE_DIR}/..)
add_dependencies(luabind luajit)