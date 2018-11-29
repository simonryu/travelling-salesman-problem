cmake_minimum_required(VERSION 3.9)
include(${PROJECT_SOURCE_DIR}/utils.cmake)

# Project
set(TARGET_NAME solver_c)
set(LANGUAGES C)

#Package(s)
find_package(OpenMP REQUIRED)

# Source(s)
set(SOURCE_FILES
        main.c
)

# Include(s)
set(INCLUDE_DIRS
        src
)

# Compiler flag(s) and option(s)
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS} -Wall -Werror -Wno-format -Wno-incompatible-pointer-types -Wno-pointer-to-int-cast")
set(CMAKE_C_FLAGS_DEBUG  "-g -DDEBUG")
set(CMAKE_C_FLAGS_RELEASE "-O3 -DNDEBUG")

if(AUTOMATIC_VECTORIZE)
    set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -ftree-vectorize")
endif()

# Target(s)
add_executable(${TARGET_NAME} ${SOURCE_FILES})
target_include_directories(${TARGET_NAME} PRIVATE ${INCLUDE_DIRS})
print_configuration(
        ${TARGET_NAME}
        ${CMAKE_C_FLAGS}
        ${CMAKE_C_COMPILER_ID}
        ${CMAKE_C_COMPILER_VERSION}
        ${LANGUAGES}
        ${CMAKE_C_STANDARD}
)

# End