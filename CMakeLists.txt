cmake_minimum_required(VERSION 3.27)
project(learn_opengl)

set(CMAKE_CXX_STANDARD 20)

add_definitions(-DDEBUG)

file(GLOB ASSETS "assets" )
file(COPY ${ASSETS} DESTINATION ${CMAKE_BINARY_DIR})

file(GLOB_RECURSE GLOBAL_SRC ${CMAKE_SOURCE_DIR}/global/*.cpp)
file(GLOB_RECURSE APPLICATION_SRC ${CMAKE_SOURCE_DIR}/application/*.cpp)
file(GLOB_RECURSE GRAPHICS_SRC ${CMAKE_SOURCE_DIR}/graphics/*.cpp)

find_package(OpenGL REQUIRED)
if (NOT OpenGL_FOUND)
    message(FATAL_ERROR "OpenGL not found!")
endif()

add_subdirectory(ext/glfw)
add_subdirectory(ext/glad)
add_subdirectory(ext/glm)
add_subdirectory(ext/stb_image)
add_subdirectory(ext/assimp)
add_subdirectory(ext/imgui)

include_directories(ext/assimp/include)
include_directories(build/ext/assimp/include)

include_directories(ext/glad/include)
include_directories(ext/glfw/include)
include_directories(ext/stb_image/include)
include_directories(ext/imgui/include)
include_directories(ext/glm)

include_directories(
    ${CMAKE_SOURCE_DIR}
    ${OPENGL_INCLUDE_DIR}
)

set(SOURCES
    ${APPLICATION_SRC}
    ${GRAPHICS_SRC}
    ${GLOBAL_SRC}

	${CMAKE_SOURCE_DIR}/main.cpp 
)

# 定义可执行目标
add_executable(learn_opengl ${SOURCES})

target_link_libraries(learn_opengl 
glfw
glad
stb_image
assimp
glm
imgui
${OPENGL_LIBRARIES} 
)



