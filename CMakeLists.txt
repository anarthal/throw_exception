# Copyright 2019 Peter Dimov
# Distributed under the Boost Software License, Version 1.0.
# See accompanying file LICENSE_1_0.txt or copy at http://www.boost.org/LICENSE_1_0.txt

cmake_minimum_required(VERSION 3.5...3.31)
project(boost_throw_exception VERSION "${BOOST_SUPERPROJECT_VERSION}" LANGUAGES CXX)

if (BOOST_USE_MODULES)
  add_library(boost_throw_exception STATIC)
  boost_set_cxx20_module_settings(boost_throw_exception)
  target_sources(boost_throw_exception PUBLIC FILE_SET CXX_MODULES BASE_DIRS modules FILES modules/boost_throw_exception.cppm)
  set(__scope PUBLIC)
else()
  add_library(boost_throw_exception INTERFACE)
  set(__scope PRIVATE)
endif()

add_library(Boost::throw_exception ALIAS boost_throw_exception)
target_include_directories(boost_throw_exception ${__scope} include)
target_link_libraries(boost_throw_exception
  ${__scope}
    Boost::assert
    Boost::config
)

if(BUILD_TESTING)

  add_subdirectory(test)

endif()
