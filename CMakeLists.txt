cmake_minimum_required(VERSION 3.10)
project(AntSimulator VERSION 1.0.0 LANGUAGES CXX)

# Default to release builds
if(NOT CMAKE_BUILD_TYPE)
   set(CMAKE_BUILD_TYPE Release)
endif()

set(CMAKE_MODULE_PATH "/usr/lib")
find_package(SFML 2 REQUIRED COMPONENTS graphics system window)
find_package(Threads REQUIRED)

if(WIN32)
   set(WIN32_GUI WIN32)
endif()

set(SFML_LIBS sfml-graphics sfml-system sfml-window )

file(GLOB SOURCES src/*.cpp)
add_executable(${PROJECT_NAME} ${WIN32_GUI} ${SOURCES})
target_include_directories(${PROJECT_NAME} PRIVATE include lib)
target_link_libraries(${PROJECT_NAME} ${SFML_LIBS} Threads::Threads)
target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_11)

# Copy res dir to the binary directory
file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/res DESTINATION ${CMAKE_CURRENT_BINARY_DIR})

if(MSVC)
   foreach(lib ${SFML_LIBS})
      get_target_property(lib_path ${lib} LOCATION)
      file(COPY ${lib_path} DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
   endforeach()
endif()
