cmake_minimum_required(VERSION 3.11 FATAL_ERROR)


project(lGPIOs VERSION 1.0 DESCRIPTION "Library to control the GPIOs " LANGUAGES C)

set(default_build_type "Release")
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
	message(STATUS "Setting build type to '${default_build_type}' as none was specified.")
	set(CMAKE_BUILD_TYPE "${default_build_type}" CACHE
		STRING "Choose the type of build." FORCE)
	# Set the possible values of build type for cmake-gui
	set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS
		"Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

include(GNUInstallDirs)

file(GLOB LGPIO_SRC
	"src/*.c"
	)

add_library(lgpio ${LGPIO_SRC})


target_include_directories(lgpio PUBLIC  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
	"$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/lgpio>")


install(TARGETS lgpio EXPORT lgpioTargets
	DESTINATION ${CMAKE_INSTALL_LIBDIR}
	)
install(DIRECTORY include/
	DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/lgpio
	)

install(EXPORT lgpioTargets
	FILE lgpioTargets.cmake
	DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/lgpio
	)

include(CMakePackageConfigHelpers)
configure_package_config_file(${CMAKE_CURRENT_SOURCE_DIR}/Config.cmake.in
	"${CMAKE_CURRENT_BINARY_DIR}/lgpioConfig.cmake"
	INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/lgpio
	NO_SET_AND_CHECK_MACRO
	NO_CHECK_REQUIRED_COMPONENTS_MACRO
	)
write_basic_package_version_file(
	"${CMAKE_CURRENT_BINARY_DIR}/lgpioConfigVersion.cmake"
	VERSION 1.0.0
	COMPATIBILITY SameMajorVersion
	)
install(FILES
	${CMAKE_CURRENT_BINARY_DIR}/lgpioConfig.cmake
	${CMAKE_CURRENT_BINARY_DIR}/lgpioConfigVersion.cmake
	DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/lgpio
	)
export(EXPORT lgpioTargets
	FILE "${CMAKE_CURRENT_BINARY_DIR}/lgpioTargets.cmake"
	)

