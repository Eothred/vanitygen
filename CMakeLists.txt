cmake_minimum_required(VERSION 2.8.12 FATAL_ERROR)
#set(CMAKE_LEGACY_CYGWIN_WIN32 0)

# Set the project name and options
project(Vanitygen C)


#TODO only set root directory when OSX/Homebrew?
set(OPENSSL_ROOT_DIR /usr/local/opt/openssl)

find_package(OpenSSL REQUIRED)
include_directories(${OPENSSL_INCLUDE_DIR})

# TODO: PCRE properly

include_directories(/usr/local/Cellar/pcre/8.41/include)
link_directories(/usr/local/Cellar/pcre/8.41/lib/)

#TODO: OpenCL only for OSX
find_package(OpenCL REQUIRED)
include_directories(${OPENCL_INCLUDE_DIRS})

# TODO seems we need curl?
find_package(CURL REQUIRED)
include_directories(${CURL_INCLUDE_DIRS})


add_executable(vanitygen vanitygen pattern util)
target_link_libraries(vanitygen ${OPENSSL_LIBRARIES} pcre m pthread)

add_executable(keyconv keyconv util)
target_link_libraries(keyconv ${OPENSSL_LIBRARIES} pcre m pthread)

add_executable(oclvanitygen oclvanitygen oclengine pattern util)
target_link_libraries(oclvanitygen ${OPENSSL_LIBRARIES} ${OpenCL_LIBRARIES} pcre m pthread)

add_executable(oclvanityminer oclvanityminer oclengine pattern util)
target_link_libraries(oclvanityminer ${OPENSSL_LIBRARIES} ${OpenCL_LIBRARIES} ${CURL_LIBRARIES} pcre m pthread)


