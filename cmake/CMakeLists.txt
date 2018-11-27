cmake_minimum_required(VERSION 3.2)

project(wxSQLite3_SEE VERSION 4.3.0)

message(STATUS "--------------------- wxSQLite3_SEE ---------------------")
message(STATUS "Please note that the rijndael.* files are licensed under" )
message(STATUS "the LGPL-3.0+ WITH WxWindows-exception-3.1. Make sure to" )
message(STATUS "read and respect the license terms, if linking statically")
message(STATUS "with the sqlite3lib target."                              )
message(STATUS "---------------------------------------------------------")

#file(GLOB SQLITE_SEE_SRC ${CMAKE_CURRENT_SOURCE_DIR}/sqlite3secure/src/*.c)
#file(GLOB SQLITE_SEE_HDR ${CMAKE_CURRENT_SOURCE_DIR}/sqlite3secure/src/*.h)

#sqlite3.def 	Module definition specifying exported functions
#sqlite3.rc 	Resource file specifying version information for the SQLite3 library
#sqlite3shell.rc 	Resource file specifying version information for the SQLite3 shell

list(APPEND SQLITE3SECURE_DLL_SRC "${CMAKE_CURRENT_SOURCE_DIR}/../sqlite3secure/src/sqlite3secure.c")
list(APPEND SQLITE3SECURE_LIB_SRC "${CMAKE_CURRENT_SOURCE_DIR}/../sqlite3secure/src/sqlite3secure.c")
list(APPEND SQLITE3SECURE_SHL_SRC "${CMAKE_CURRENT_SOURCE_DIR}/../sqlite3secure/src/shell.c")

if (MSVC) 
  list(APPEND SQLITE3SECURE_DLL_SRC "${CMAKE_CURRENT_SOURCE_DIR}/../sqlite3secure/src/sqlite3.rc")
  list(APPEND SQLITE3SECURE_DLL_SRC "${CMAKE_CURRENT_SOURCE_DIR}/../sqlite3secure/src/sqlite3.def")
  list(APPEND SQLITE3SECURE_LIB_SRC "${CMAKE_CURRENT_SOURCE_DIR}/../sqlite3secure/src/sqlite3.rc")
  list(APPEND SQLITE3SECURE_SHL_SRC "${CMAKE_CURRENT_SOURCE_DIR}/../sqlite3secure/src/sqlite3shell.rc")
endif()

add_library(sqlite3dll SHARED ${SQLITE3SECURE_DLL_SRC})
add_library(sqlite3lib STATIC ${SQLITE3SECURE_LIB_SRC})
add_executable(sqlite3shell ${SQLITE3SECURE_SHL_SRC})
target_link_libraries(sqlite3shell sqlite3lib)

target_include_directories(sqlite3dll PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/../sqlite3secure/src)
target_compile_definitions(sqlite3dll PRIVATE 
  _USRDLL
  THREADSAFE=1
  SQLITE_MAX_ATTACHED=10
  SQLITE_SOUNDEX
  SQLITE_ENABLE_COLUMN_METADATA
  SQLITE_HAS_CODEC=1
  CODEC_TYPE=CODEC_TYPE_CHACHA20
  SQLITE_SECURE_DELETE
  SQLITE_ENABLE_FTS3
  SQLITE_ENABLE_FTS3_PARENTHESIS
  SQLITE_ENABLE_FTS4
  SQLITE_ENABLE_FTS5
  SQLITE_ENABLE_JSON1
  SQLITE_ENABLE_RTREE
  SQLITE_ENABLE_GEOPOLY
  SQLITE_CORE
  SQLITE_ENABLE_EXTFUNC
  SQLITE_ENABLE_CSV
  SQLITE_ENABLE_SHA3
  SQLITE_ENABLE_CARRAY
  SQLITE_ENABLE_FILEIO
  SQLITE_ENABLE_SERIES
  SQLITE_TEMP_STORE=2
  SQLITE_USE_URI
  SQLITE_USER_AUTHENTICATION
)

target_include_directories(sqlite3lib PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/../sqlite3secure/src)
target_compile_definitions(sqlite3lib PRIVATE 
  _LIB
  THREADSAFE=1
  SQLITE_MAX_ATTACHED=10
  SQLITE_ENABLE_EXPLAIN_COMMENTS
  SQLITE_SOUNDEX
  SQLITE_ENABLE_COLUMN_METADATA
  SQLITE_HAS_CODEC=1
  CODEC_TYPE=CODEC_TYPE_CHACHA20
  SQLITE_SECURE_DELETE
  SQLITE_ENABLE_FTS3
  SQLITE_ENABLE_FTS3_PARENTHESIS
  SQLITE_ENABLE_FTS4
  SQLITE_ENABLE_FTS5
  SQLITE_ENABLE_JSON1
  SQLITE_ENABLE_RTREE
  SQLITE_ENABLE_GEOPOLY
  SQLITE_CORE
  SQLITE_ENABLE_EXTFUNC
  SQLITE_ENABLE_CSV
#  SQLITE_ENABLE_SHA3
  SQLITE_ENABLE_CARRAY
#  SQLITE_ENABLE_FILEIO
  SQLITE_ENABLE_SERIES
  SQLITE_TEMP_STORE=2
  SQLITE_USE_URI
  SQLITE_USER_AUTHENTICATION
# Compatibility with official SQLite3 shell    
  SQLITE_ENABLE_DBSTAT_VTAB
  SQLITE_ENABLE_STMTVTAB
  SQLITE_ENABLE_UNKNOWN_SQL_FUNCTION
)

target_include_directories(sqlite3shell PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/../sqlite3secure/src)
target_compile_definitions(sqlite3shell PRIVATE 
  SQLITE_SHELL_IS_UTF8
  SQLITE_HAS_CODEC=1
  SQLITE_USER_AUTHENTICATION
)
