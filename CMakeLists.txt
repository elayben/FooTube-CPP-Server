cmake_minimum_required(VERSION 3.16)
project(FooTube_CPP_Server CXX)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Threads REQUIRED)

# Recommendation server (multithreaded TCP, port 5555)
add_executable(Server Server.cpp)
target_link_libraries(Server PRIVATE Threads::Threads)

# Interactive test client
add_executable(Client Client.cpp)
