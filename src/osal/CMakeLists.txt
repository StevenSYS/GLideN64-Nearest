cmake_minimum_required(VERSION 3.10)

project( osal )

LINK_DIRECTORIES( ${CMAKE_CURRENT_SOURCE_DIR}/lib )

if(UNIX)
  if(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    set(OSAL_SOURCES osal_files_unix.c osal_keys_linux.c)
    add_definitions(
      -DOS_LINUX -DNDEBUG
    )
  else()
    set(OSAL_SOURCES osal_files_unix.c osal_keys_unix.c)
    add_definitions(
     -DNDEBUG
    )
  endif()
endif(UNIX)

if(WIN32)
  set(OSAL_SOURCES osal_files_win32.c osal_keys_win.c)
  add_definitions(
	-DOS_WINDOWS
	-D_CRT_SECURE_NO_WARNINGS
  )
endif(WIN32)

# Build type

if( GLIDEN64_BUILD_TYPE STREQUAL "Debug")
	set( DEBUG_BUILD TRUE)
	add_definitions(
		-DDEBUG
	)
endif( GLIDEN64_BUILD_TYPE STREQUAL "Debug")

if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "MSVC")
	add_definitions( -D__MSC__)
endif()

if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU" OR "${CMAKE_CXX_COMPILER_ID}" MATCHES "Clang")
    SET(GCC_CPP11_COMPILE_FLAGS "-std=c++0x")
    if (NOT MINGW)
        SET( PIC_FLAGS "-fPIC")
    endif(NOT MINGW)
    SET( CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} ${GCC_CPP11_COMPILE_FLAGS} -static ${PIC_FLAGS} " )
    SET( CMAKE_C_FLAGS  "${CMAKE_C_FLAGS} -static ${PIC_FLAGS} " )
endif()

if( GLIDEN64_BUILD_TYPE STREQUAL "Debug")
  add_library( osald STATIC ${OSAL_SOURCES})
else( GLIDEN64_BUILD_TYPE STREQUAL "Debug")
  add_library( osal STATIC ${OSAL_SOURCES})
endif( GLIDEN64_BUILD_TYPE STREQUAL "Debug")
