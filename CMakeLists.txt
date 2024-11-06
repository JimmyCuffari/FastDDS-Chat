cmake_minimum_required(VERSION 3.20)

project(FastDDSChat)# VERSION 3.1.0 LANGUAGES C CXX)

add_library(FastDDSChat SHARED src/FastDDSUser.cpp src/FastDDSChat.def)


# Windows
set(OPENSSL_ROOT_DIR "C:/Program Files/OpenSSL-Win64/")
set(OPENSSL_INCLUDE_DIR "C:/Program Files/OpenSSL-Win64/include")
set(OPENSSL_LIBRARIES "C:/Program Files/OpenSSL-Win64/lib")

#set(OPENSSL_CRYPTO_LIBRARY "C:/Program Files/OpenSSL-Win64/lib/VC/x64/MD/libcrypto.lib")
#set(OPENSSL_SSL_LIBRARY "C:/Program Files/OpenSSL-Win64/lib/VC/x64/MD/libssl.lib")


#set(OPENSSL_ROOT_DIR "/usr")
#set(OPENSSL_INCLUDE_DIR "/usr/include/openssl")
#set(OPENSSL_CRYPTO_LIBRARY "/usr/bin/openssl")
#set(OPENSSL_LIBRARIES "/usr/lib/x86_64-linux-gnu/libssl.so;/usr/lib/x86_64-linux-gnu/libcrypto.so")

find_package(OpenSSL REQUIRED)

# Find Requirements
if (NOT fastcdr_FOUND)
    find_package(fastcdr 2 REQUIRED)
endif()

if (NOT fastdds_FOUND)
    find_package(fastdds 3 REQUIRED)
endif()

# Set C++11
include(CheckCXXCompilerFlag)

if (CMAKE_COMPILER_IS_GNUCXX OR CMAKE_COMPILER_IS_CLANG OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    check_cxx_compiler_flag(-std=c++11 SUPPORTS_CXX11)
    if (SUPPORTS_CXX11)
        add_compile_options(-std=c++11)
    else()
        message(FATAL_ERROR "Compiler doesn't support C++11")
    endif()
endif()

#find_package(OPENSSL REQUIRED)
#include_directories(${OPENSSL_INCLUDE_DIR})

message(STATUS "Configuring FastDDS Chat...")
file(GLOB FASTDDS_CHAT_SOURCES_CXX "src/*.cxx")

#add_executable(DDSHelloWorldPublisher src/HelloWorldPublisher.cpp ${DDS_HELLOWORLD_SOURCES_CXX})
#target_link_libraries(DDSHelloWorldPublisher fastdds fastcdr)

#add_executable(DDSHelloWorldSubscriber src/HelloWorldSubscriber.cpp ${DDS_HELLOWORLD_SOURCES_CXX})
#target_link_libraries(DDSHelloWorldSubscriber fastdds fastcdr)


add_executable(FastDDSUser src/FastDDSUser.cpp ${FASTDDS_CHAT_SOURCES_CXX})


set_target_properties(FastDDSChat PROPERTIES
    WINDOWS_EXPORT_ALL_SYMBOLS ON  # Export all symbols in the DLL
    OUTPUT_NAME "fastddschat"       # Optional: name of the DLL file
)


target_link_libraries(FastDDSUser fastdds fastcdr OpenSSL::SSL OpenSSL::Crypto)