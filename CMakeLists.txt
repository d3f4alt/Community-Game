# Specify the minimum CMAKE version required
cmake_minimum_required(VERSION 2.8)

# Your project's name
project(Community-Game)

# Make your header file accessible to the compiler
include_directories(include)

# Add all files from the source/ folder to CMake
file(GLOB_RECURSE SRC
    "src/*.cpp"
    "src/*.h"
    "src/*.hpp" 
)

# Define the executable
add_executable(${PROJECT_NAME} ${SRC})

# SFML include
if(WIN32)
    include_directories(SFML/include)
    add_definitions(-DSFML_STATIC)

    set(SFML_BUILD_DOC OFF CACHE BOOL "" FORCE)
    set(SFML_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
    set(BUILD_SHARED_LIBS OFF CACHE BOOL "" FORCE)
    add_subdirectory(SFML)

    target_link_libraries(${PROJECT_NAME} sfml-network sfml-graphics sfml-window sfml-system)
elseif(UNIX)
    set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake_modules")

	find_package(SFML REQUIRED system window graphics network)
	if(SFML_FOUND)
	    include_directories(${SFML_INCLUDE_DIR})
	    target_link_libraries(${PROJECT_NAME} ${SFML_LIBRARIES})
	endif()
endif()

# Enable C++ 14
set_target_properties(${PROJECT_NAME} PROPERTIES
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
)
