cmake_minimum_required(VERSION 3.22)
project(performance_consumer_cpp)

set(CMAKE_CXX_STANDARD 23)
set(TARGET performance_consumer_cpp)

FILE(GLOB Sources *.cpp)
add_executable(${TARGET} ${Sources})

if (UNIX AND NOT APPLE)
    add_definitions(-D_GLIBCXX_USE_CXX11_ABI=0)
    include_directories(/usr/include/pulsar)
    target_link_libraries(${TARGET} /usr/lib/libpulsar.so)
elseif(UNIX)
    if (EXISTS usr/local/Cellar/libpulsar)
        include_directories(/usr/local/Cellar/libpulsar/2.10.1/include)
        target_link_libraries(${TARGET} /usr/local/Cellar/libpulsar/2.10.1/lib/libpulsar.dylib)
    else()
        include_directories(/opt/homebrew/Cellar/libpulsar/2.10.1/include)
        target_link_libraries(${TARGET} /opt/homebrew/Cellar/libpulsar/2.10.1/lib/libpulsar.dylib)
    endif()
endif()
