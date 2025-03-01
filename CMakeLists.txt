cmake_minimum_required(VERSION 3.10)

# set the project name
project(Bedhock)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)
set(CMAKE_REQUIRED_FLAGS "-std=c++17")
set(SUBHOOK_STATIC ON)
set(BUILD_SHARED_LIBS ON)
set(CMAKE_CXX_FLAGS "-fPIC -g -Wno-narrowing")
set(CMAKE_C_FLAGS "-fPIC -g -Wno-narrowing")
set(CMAKE_BUILD_TYPE Debug)

add_subdirectory(subhook)
add_subdirectory(fmt)

# Find source files
file(GLOB SOURCES  src/*.cpp src/hooks/*.cpp src/wrap/*.cpp src/json/*.cpp src/fakemine/*.cpp)

find_package(PkgConfig REQUIRED)
pkg_check_modules(JSONCPP jsoncpp)
pkg_check_modules(LIBZMQ libzmq)

add_library(${PROJECT_NAME} SHARED ${SOURCES})

# Include header files
include_directories(include)

target_link_libraries(${PROJECT_NAME} subhook)
target_link_libraries(${PROJECT_NAME} fmt)
link_libraries(${JSONCPP_LIBRARIES})
link_libraries(${LIBZMQ_LIBRARIES})
target_link_libraries(${PROJECT_NAME} ${JSONCPP_LIBRARIES})
target_link_libraries(${PROJECT_NAME} ${LIBZMQ_LIBRARIES})

target_include_directories(${PROJECT_NAME} PRIVATE include)
target_include_directories(${PROJECT_NAME} PRIVATE src)
target_include_directories(${PROJECT_NAME} PRIVATE ELFIO)
target_include_directories(${PROJECT_NAME} PRIVATE cppzmq)
target_include_directories(${PROJECT_NAME} PRIVATE SafeQueue)
target_include_directories(${PROJECT_NAME} PRIVATE gsl)
target_include_directories(${PROJECT_NAME} PRIVATE fmt/include)

set_target_properties(${PROJECT_NAME} PROPERTIES VERSION 1.0)
set_target_properties(${PROJECT_NAME} PROPERTIES VERSION_MAJOR 1)
set_target_properties(${PROJECT_NAME} PROPERTIES VERSION_MINOR 0)
set_target_properties(${PROJECT_NAME} PROPERTIES DESCRIPTION "Bedrock Server Hooking")
set_target_properties(${PROJECT_NAME} PROPERTIES SOVERSION 1)

# Install library headers
file(GLOB HEADERS include/*.h)
install(FILES ${HEADERS} DESTINATION include/${PROJECT_NAME})