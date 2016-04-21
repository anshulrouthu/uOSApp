
cmake_minimum_required(VERSION 2.8.4)
project(uOSapp)

set(CMAKE_BUILD_TYPE "Debug" CACHE STRING "Choose the type of build, options are: Debug Release." FORCE)

# Helper macros
macro(addtolist list file condition)
    if(${condition})
        list(APPEND ${list} ${file})
    endif()
endmacro()

macro(addtodefinition def)
    if(${def})
        add_definitions(-D${def})
    endif()
endmacro()

# Set option required for the project
option(ENABLE_MEMMGR "Enable memory manager" ON)
option(ENABLE_TSK_SCHEDULER "Enable task scheduler" ON)

addtodefinition(ENABLE_MEMMGR)
addtodefinition(ENABLE_TSK_SCHEDULER)

add_subdirectory(core)
get_directory_property(CORE_INC_DIR DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/core DEFINITION PUBLIC_INC_DIR)
get_directory_property(CORE_LIB_NAME DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/core DEFINITION LIB_NAME)

# Include dirs  list
list(APPEND SRC_INC_DIR ${CMAKE_CURRENT_SOURCE_DIR}/source)

# Source files
addtolist(SRC_FILES ${CMAKE_CURRENT_SOURCE_DIR}/source/main.cpp  "ON")

# Include directories
include_directories(${CMAKE_CURRENT_SOURCE_DIR} ${SRC_INC_DIR} ${CORE_INC_DIR})

add_executable(${PROJECT_NAME} ${SRC_FILES})
add_dependencies(${PROJECT_NAME} ${CORE_LIB_NAME})
target_link_libraries(${PROJECT_NAME} ${CORE_LIB_NAME})