# Copyright 2019 Peter Dimov
# Distributed under the Boost Software License, Version 1.0.
# See accompanying file LICENSE_1_0.txt or copy at http://www.boost.org/LICENSE_1_0.txt

# We support CMake 3.5, but prefer 4.3 policies and behavior
cmake_minimum_required(VERSION 3.5...4.3)

project(boost_throw_exception VERSION "${BOOST_SUPERPROJECT_VERSION}" LANGUAGES CXX)

add_library(boost_throw_exception INTERFACE)
add_library(Boost::throw_exception ALIAS boost_throw_exception)

target_link_libraries(boost_throw_exception
  INTERFACE
    Boost::assert
    Boost::config
)

if(BOOST_USE_MODULES)
  # This library exports only headers even with BOOST_USE_MODULES,
  # but consuming it requires 'import std' and defining BOOST_USE_MODULES
  target_compile_features(boost_throw_exception INTERFACE cxx_std_23)
  target_compile_definitions(boost_throw_exception INTERFACE BOOST_USE_MODULES)
endif()

# -----------------------------------------------------------------------------
# Include headers
# -----------------------------------------------------------------------------
if(NOT BOOST_USE_MODULES)
  target_include_directories(boost_throw_exception INTERFACE include)
endif()

# Note: usable starting with cmake v3.23
if(NOT CMAKE_VERSION VERSION_LESS 3.23)
  target_sources(
    boost_throw_exception
    PUBLIC
      FILE_SET headers_public
        TYPE HEADERS
        BASE_DIRS include
        FILES
          include/boost/throw_exception.hpp
          include/boost/exception/exception.hpp
  )
endif()

if(BUILD_TESTING)

  add_subdirectory(test)

endif()
