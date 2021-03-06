CMAKE_MINIMUM_REQUIRED(VERSION 3.5)
PROJECT(ShouYou)

INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR}/deps/)
INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR}/deps/boost_1_58)
INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR}/deps/sdk)
INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR}/deps/pugixml/src)
INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR}/message)
INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR}/deps/google)

SET(LINK_DIRECTORIES
  ${CMAKE_SOURCE_DIR}/deps/boost/lib
  ${CMAKE_SOURCE_DIR}/deps/google/lib
  ${CMAKE_SOURCE_DIR}/bin
  )
SET(CMAKE_LIBRARY_PATH
  ${CMAKE_SOURCE_DIR}/deps/boost/lib
  ${CMAKE_SOURCE_DIR}/bin
  )

SET(BOOST_ROOT ${CMAKE_SOURCE_DIR}/deps/boost_1_58)
SET(Boost_USE_STATIC_LIBS ON)
FIND_PACKAGE(Boost COMPONENTS system filesystem thread REQUIRED)
MESSAGE(STATUS ${Boost_SYSTEM_LIBRARY})
MESSAGE(STATUS ${Boost_FILESYSTEM_LIBRARY})
MESSAGE(STATUS ${Boost_THREAD_LIBRARY})

SET(MYSQLCLIENT_LIBRARY ${CMAKE_SOURCE_DIR}/bin/libmysqlclient_r.so)
MESSAGE(${MYSQLCLIENT_LIBRARY})

SET(ALLOCATOR_LIBRARY ${CMAKE_SOURCE_DIR}/bin/libjemalloc.so)
MESSAGE(STATUS ${ALLOCATOR_LIBRARY})

SET(PROTOBUF_LIBRARY ${CMAKE_SOURCE_DIR}/deps/google/lib/libprotobuf.a)
MESSAGE(STATUS ${PROTOBUF_LIBRARY})

SET(LEVELDB_LIBRARY ${CMAKE_SOURCE_DIR}/deps/google/lib/libleveldb.a)
MESSAGE(STATUS ${LEVELDB_LIBRARY})

SET(CMAKE_SKIP_BUILD_RPATH  TRUE)
SET(CMAKE_BUILD_WITH_INSTALL_RPATH TRUE)
SET(CMAKE_INSTALL_RPATH ".")
SET(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

SET(EXECUTABLE_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/bin)

IF(NOT CMAKE_BUILD_TYPE)
  MESSAGE(STATUS "DEBUG")
  SET(CMAKE_BUILD_TYPE "Debug")
ENDIF()

IF(EGMKANG_DEBUG)
  ADD_DEFINITIONS(-D__EGMKANG_DEBUG)
ENDIF()
IF(INTERNAL_DEBUG)
  ADD_DEFINITIONS(-D__INTERNAL_DEBUG)
ENDIF()
IF(IOS_PLATFORM)
  ADD_DEFINITIONS(-DIOS_PLATFORM)
ENDIF()

#SET(CMAKE_CXX_COMPILER "g++")

SET(CXX_FLAGS
    -g
    -Wall
    -Wextra
    -Wno-unused-parameter
    -Wno-unused-local-typedefs
    -Wno-unused-result
    -fomit-frame-pointer
    #-fsanitize=address
    -D_GLIBCXX_USE_CXX11_ABI=0
    -pthread
    -fstack-protector-all
    -DRAPIDJSON_HAS_STDSTRING
    -DPUGIXML_HAS_LONG_LONG
    )

STRING(REPLACE ";" " " CMAKE_CXX_FLAGS "${CXX_FLAGS}")
MESSAGE(STATUS "CXX flags : " "${CMAKE_CXX_FLAGS}")

SET(CMAKE_CXX_FLAGS_DEBUG   "-DDEBUG -D_DEBUG -O0")
SET(CMAKE_CXX_FLAGS_RELEASE "-DNDEBUG -finline-small-functions -finline-limit=1000 -finline-functions")


ADD_SUBDIRECTORY(deps)
ADD_SUBDIRECTORY(message)
ADD_SUBDIRECTORY(LogicServer)
ADD_SUBDIRECTORY(RecordServer)
ADD_SUBDIRECTORY(AuthServer)
ADD_SUBDIRECTORY(CenterServer)

