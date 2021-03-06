cmake_minimum_required(VERSION 3.1)
project(MahjongPy)

set(LIB_MahjongPy MahjongPy)
file(GLOB_RECURSE MahjongPy_SRC_CPP *.cpp)
add_library(${LIB_MahjongPy} SHARED ${MahjongPy_SRC_CPP})
if (UNIX)
    if (APPLE)
        set_target_properties(${LIB_MahjongPy} PROPERTIES SUFFIX ".so")
    endif(APPLE)
    set_target_properties(${LIB_MahjongPy} PROPERTIES PREFIX "")
elseif(WIN32)
    if (NOT MSVC)
        set_target_properties(${LIB_MahjongPy} PROPERTIES PREFIX "")
    endif()
    set_target_properties(${LIB_MahjongPy} PROPERTIES SUFFIX ".pyd")
endif(UNIX)

target_link_libraries(${LIB_MahjongPy} Mahjong MahjongAlgorithm ${PYTHON_LIBRARIES})

set(LIB_MahjongPy_OUT_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../AI")
if (UNIX)
    set(LIB_MahjongPy_OUT_LIB "${LIB_MahjongPy}.so")
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${LIBRARY_OUTPUT_PATH})
elseif (WIN32)
    set(LIB_MahjongPy_OUT_LIB "${LIB_MahjongPy}.pyd")
endif(UNIX)

add_custom_command(OUTPUT ${LIB_MahjongPy_OUT_LIB}
   COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE}/${LIB_MahjongPy_OUT_LIB} ${LIB_MahjongPy_OUT_PATH}/${LIB_MahjongPy_OUT_LIB}
 )
add_custom_target(CopyTask ALL DEPENDS ${LIB_MahjongPy_OUT_LIB} ${LIB_MahjongPy})
