# -----------------------------------------------------------------------------
# CppAD: C++ Algorithmic Differentiation: Copyright (C) 2003-17 Bradley M. Bell
#
# CppAD is distributed under multiple licenses. This distribution is under
# the terms of the
#                     Eclipse Public License Version 1.0.
#
# A copy of this license is included in the COPYING file of this distribution.
# Please visit http://www.coin-or.org/CppAD/ for information on other licenses.
# -----------------------------------------------------------------------------
# Build the example/multi_thread/openmp directory tests
# Inherit build type from ../CMakeList.txt

# Local include directories to search (not in package_prefix/include)
INCLUDE_DIRECTORIES( ${CMAKE_CURRENT_SOURCE_DIR}/.. )

# Cannot find specifications for CMAKE_CXX_FLAGS in version 2.6 documentation
# so using ADD_DEFINITIONS instead.
ADD_DEFINITIONS( ${OpenMP_CXX_FLAGS} )

# add_executable(<name> [WIN32] [MACOSX_BUNDLE] [EXCLUDE_FROM_ALL]
#                 source1 source2 ... sourceN
# )
SET(source_list ../thread_test.cpp
	${CMAKE_SOURCE_DIR}/speed/src/microsoft_timer.cpp
	../team_example.cpp
	../harmonic.cpp
	../multi_atomic.cpp
	../multi_newton.cpp
	a11c_openmp.cpp
	simple_ad_openmp.cpp
	team_openmp.cpp
)
set_compile_flags( example_multi_thread_openmp "${cppad_debug_which}" "${source_list}" )
#
ADD_EXECUTABLE( example_multi_thread_openmp EXCLUDE_FROM_ALL ${source_list} )

# Extra flags used by linker for openmp support
SET(CMAKE_EXE_LINKER_FLAGS ${OpenMP_CXX_FLAGS} )

# Add the check_example_multi_thread_openmp target
ADD_CUSTOM_TARGET(check_example_multi_thread_openmp
	example_multi_thread_openmp simple_ad
	DEPENDS example_multi_thread_openmp
)
MESSAGE(STATUS "make check_example_multi_thread_openmp: available")


# Change check depends in parent environment
add_to_list(check_example_multi_thread_depends check_example_multi_thread_openmp)
SET(check_example_multi_thread_depends "${check_example_multi_thread_depends}" PARENT_SCOPE)
