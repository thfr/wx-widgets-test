cmake_minimum_required(VERSION 3.16)

project(wxWidgetsTest VERSION 0.0.1)

# compilation settings
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED TRUE)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wshadow -Wall -Wextra")
# set(CMAKE_CXX_CLANG_TIDY clang-tidy -checks=-*,readability-*)
set(CMAKE_EXPORT_COMPILE_COMMANDS TRUE)

# Get thirdparties
include(FetchContent)
FetchContent_Declare(
  wxWidgets
  URL  https://github.com/wxWidgets/wxWidgets/releases/download/v3.1.4/wxWidgets-3.1.4.tar.bz2
)
FetchContent_GetProperties(wxWidgets)
string(TOLOWER "wxWidgets" lcName)
if(NOT ${lcName}_POPULATED)
  # Fetch the content using previously declared details
  FetchContent_Populate(wxWidgets)

  # Set custom variables, policies, etc.
  set(wxBUILD_SHARED FALSE)

  # Bring the populated content into the build
  add_subdirectory(${${lcName}_SOURCE_DIR} ${${lcName}_BINARY_DIR})
endif()

# declare source files
set(sources ./src/main.cpp)

configure_file(src/config.h.in config.h)

# declare targets
add_executable(${PROJECT_NAME} ${sources})
target_include_directories(${PROJECT_NAME} PUBLIC "${PROJECT_BINARY_DIR}")
target_link_libraries(${PROJECT_NAME} wx::core wx::base)
