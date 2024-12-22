# Copyright 2019 Peter Dimov
# Distributed under the Boost Software License, Version 1.0.
# See accompanying file LICENSE_1_0.txt or copy at http://www.boost.org/LICENSE_1_0.txt

cmake_minimum_required(VERSION 3.5...3.16)
project(boost_throw_exception VERSION "${BOOST_SUPERPROJECT_VERSION}" LANGUAGES CXX)

if (BOOST_CXX20_MODULE)
  add_library(boost_throw_exception)
  target_sources(boost_throw_exception PUBLIC FILE_SET CXX_MODULES FILES modules/throw_exception.cxx)
  target_include_directories(boost_throw_exception PUBLIC include)
  target_compile_features(boost_throw_exception PUBLIC cxx_std_23)
  set_target_properties(boost_throw_exception PROPERTIES CXX_MODULE_STD 1)
  target_compile_definitions(boost_throw_exception PRIVATE BOOST_CXX20_MODULE)
  target_compile_options(boost_throw_exception PUBLIC -Wno-include-angled-in-module-purview) # TODO: scope this to clang
  target_link_libraries(boost_throw_exception
    PUBLIC
      Boost::assert
      Boost::config
  )
else()
  add_library(boost_throw_exception INTERFACE)
  target_include_directories(boost_throw_exception INTERFACE include)
  target_link_libraries(boost_throw_exception
    INTERFACE
      Boost::assert
      Boost::config
  )
endif()

add_library(Boost::throw_exception ALIAS boost_throw_exception)


if(BUILD_TESTING)

  add_subdirectory(test)

endif()
