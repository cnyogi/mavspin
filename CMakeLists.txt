cmake_minimum_required(VERSION 2.8)
project("mav-spin" C CXX)

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING
        "Choose the type of build, options are: Debug, Release."
        FORCE)
endif(NOT CMAKE_BUILD_TYPE)

# Include find files
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

set(Boost_USE_STATIC_LIBS ON)

find_package( Boost 1.40 COMPONENTS program_options thread system REQUIRED )
find_package( Threads REQUIRED)
find_package( Readline REQUIRED)

INCLUDE_DIRECTORIES( ${Boost_INCLUDE_DIR} )
INCLUDE_DIRECTORIES( ${READLINE_INCLUDE_DIR})

# glob source files
file(GLOB mav-spin_SRC
    "src/*.cpp"
    )

#actual executable
add_executable(mav-spin ${mav-spin_SRC})
set(CMAKE_CXX_FLAGS_RELEASE "-DNDEBUG")
set(CMAKE_CXX_FLAGS_DEBUG " -ggdb")
set(CMAKE_CXX_FLAGS "-std=c++11 -Wno-address-of-packed-member -DMAVLINK_USE_MESSAGE_INFO")

TARGET_LINK_LIBRARIES(mav-spin ${Boost_LIBRARIES} ${CMAKE_THREAD_LIBS_INIT} ${READLINE_LIBRARY})

install(TARGETS mav-spin DESTINATION bin)
