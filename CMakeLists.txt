cmake_minimum_required(VERSION 3.7 FATAL_ERROR)
set(NAME VulkanTriangleIssuesChallenge)
project(${NAME} VERSION 0.0.1 LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(TUTORIEL_VK_DEFINES "")

if(CMAKE_SYSTEM_NAME STREQUAL "Windows")
	list(APPEND TUTORIEL_VK_DEFINES TUTORIEL_VK_OS_WINDOWS _CRT_SECURE_NO_WARNINGS NOMINMAX)
elseif(CMAKE_SYSTEM_NAME STREQUAL "Linux")
	list(APPEND TUTORIEL_VK_DEFINES TUTORIEL_VK_OS_LINUX)
endif()

add_subdirectory(external/glfw)
add_subdirectory(external/glslang)

find_package(Vulkan)
if(NOT Vulkan_FOUND)
	message(FATAL_ERROR "Could not find Vulkan")
else()
	message(STATUS "Vulkan has been found.")
endif()

set(SOURCES
	src/hellotriangle.cpp)
set(HEADERS
	src/hellotriangle.h)

add_executable(${PROJECT_NAME} src/main.cpp ${SOURCES} ${HEADERS})

target_link_libraries(${PROJECT_NAME} PRIVATE glfw glslang SPIRV Vulkan::Vulkan)

target_compile_definitions(${PROJECT_NAME} PRIVATE
	${TUTORIEL_VK_DEFINES})