cmake_minimum_required(VERSION 3.10 FATAL_ERROR)
project(FloydWarshall LANGUAGES CXX CUDA)

set (CMAKE_CXX_STANDARD 17)
set (CMAKE_CXX_STANDARD_REQUIRED ON)

file(GLOB_RECURSE FloydWarshall_SRC include/*.h src/*.cpp src/*.cu)

add_library(COMMON STATIC ${FloydWarshall_SRC})
target_include_directories(COMMON PUBLIC "${CMAKE_SOURCE_DIR}/include/")

set(main_project_dir "${CMAKE_SOURCE_DIR}/main/")
file(GLOB main_projects "${main_project_dir}/*.cpp")
foreach(main_project_path ${main_projects})
    # I used a simple string replace, to cut off .cpp.
    get_filename_component(main_project_name ${main_project_path} NAME )

    message("Main project path: ${main_project_path} and name: ${main_project_name}")

    add_executable(${main_project_name} ${main_project_path})
    target_link_libraries(${main_project_name} COMMON)

endforeach()