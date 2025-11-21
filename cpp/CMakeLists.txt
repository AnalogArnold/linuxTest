cmake_minimum_required(VERSION 3.18)

project(superfastcode LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
#set(CMAKE_CXX_FLAGS --verbose) # Uncomment for troubleshooting in building

#if (NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
#  set(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build." FORCE)
#  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
#endif()

# Detect the installed nanobind package and import it into CMake

find_package(Python 3.11 COMPONENTS Interpreter Development EXACT REQUIRED)
message("Python_FOUND:${Python_FOUND}")
message("Python_VERSION:${Python_VERSION}")
message("Python_Development_FOUND:${Python_Development_FOUND}")
message("Python_LIBRARIES:${Python_LIBRARIES}")
message("Python_Executable:${Python_EXECUTABLE}")
message("Python_Interpreter_FOUND: ${Python_Interpreter_FOUND}")
message("Python_INCLUDE_DIRS:${Python_INCLUDE_DIRS}")


# Source files
#set(SUPERFASTCODE_SOURCES
#    ray_intersection.cpp
#    render.cpp
#    test1.cpp
#)

#set(SUPERFASTCODE_HEADERS
#    eigen_types.h
#    hit_record.h
#    math_utils.h
#    ray.h
#    ray_intersection.h
#    render.h
#)

#add_executable(superfastcode ${SUPERFASTCODE_SOURCES} ${SUPERFASTCODE_HEADERS}) #if uncommented, it screams that target already exists and I can't bind to nanobind

 
execute_process(
  COMMAND "${Python_EXECUTABLE}" -m nanobind --cmake_dir
  OUTPUT_STRIP_TRAILING_WHITESPACE OUTPUT_VARIABLE nanobind_ROOT)
find_package(nanobind CONFIG REQUIRED)


file(GLOB RAYTRACER_SRC_FILES *.cpp)
#message("${RAYTRACER_SRC_FILES}")

set(CMAKE_PREFIX_PATH "home/tudent/linuxTest/cpp/Eigen")

nanobind_add_module(superfastcode MODULE ${RAYTRACER_SRC_FILES})

target_include_directories(superfastcode PRIVATE
    ${PYTHON_INCLUDE_DIR}
)

target_link_directories(superfastcode PRIVATE
    ${PYTHON_LIBRARY_DIR}
)

#target_link_libraries(superfastcode PRIVATE python311)

target_compile_options(superfastcode PRIVATE -O2)

install(TARGETS superfastcode LIBRARY DESTINATION /home/tudent/linuxTest/pyt)