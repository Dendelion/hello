# CPack Example: User-selectable Installation Components
#
# In this example, we have a simple library (hello) with an example 
# application (main). We create a binary installer that allows
# users to select which pieces will be installed: the example 
# application, the library binaries, and/or the header file.
cmake_minimum_required(VERSION 2.6.0 FATAL_ERROR)
project(hello)

# Create the hello library
add_library(hello hello.cpp)

# Create the main application
add_executable(main main.cpp)
target_link_libraries(main hello)

# Create installation targets. Note that we put each kind of file
# into a different component via COMPONENT. These components will 
# be used to create the installation components.
install(TARGETS hello 
  ARCHIVE
  DESTINATION lib
  COMPONENT libraries)
install(TARGETS main
  RUNTIME
  DESTINATION bin
  COMPONENT applications)
install(FILES hello.h
  DESTINATION include
  COMPONENT headers)
  
# CPack boilerplate for this project
set(CPACK_PACKAGE_NAME "hello")
set(CPACK_PACKAGE_VENDOR "CMake.org")
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "hello - CPack Component Installation Example")
set(CPACK_PACKAGE_VERSION "1.0.0")
set(CPACK_PACKAGE_VERSION_MAJOR "1")
set(CPACK_PACKAGE_VERSION_MINOR "0")
set(CPACK_PACKAGE_VERSION_PATCH "0")
set(CPACK_PACKAGE_INSTALL_DIRECTORY "CPack Component Example")

# Tell CPack all of the components to install. The "ALL"
# refers to the fact that this is the set of components that
# will be included when CPack is instructed to put everything
# into the binary installer (the default behavior).
set(CPACK_COMPONENTS_ALL applications libraries headers)

# Set the displayed names for each of the components to install.
# These will be displayed in the list of components inside the installer.
set(CPACK_COMPONENT_APPLICATIONS_DISPLAY_NAME "hello Application")
set(CPACK_COMPONENT_LIBRARIES_DISPLAY_NAME "Libraries")
set(CPACK_COMPONENT_HEADERS_DISPLAY_NAME "C++ Headers")

# Provide descriptions for each of the components to install.
# When the user hovers the mouse over the name of a component, 
# the description will be shown in the "Description" box in the
# installer. If no descriptions are provided, the "Description" 
# box will be removed.
set(CPACK_COMPONENT_APPLICATIONS_DESCRIPTION 
  "An extremely useful application that makes use of hello")
set(CPACK_COMPONENT_LIBRARIES_DESCRIPTION
  "Static libraries used to build programs with hello")
set(CPACK_COMPONENT_HEADERS_DESCRIPTION
  "C/C++ header files for use with hello")

# Put the components into two different groups: "Runtime" and "Development"
set(CPACK_COMPONENT_APPLICATIONS_GROUP "Runtime")
set(CPACK_COMPONENT_LIBRARIES_GROUP "Development")
set(CPACK_COMPONENT_HEADERS_GROUP "Development")

# Expand the "Development" group by default, since we have so few components.
# Also, provide this group with a description.
set(CPACK_COMPONENT_GROUP_DEVELOPMENT_EXPANDED ON)
set(CPACK_COMPONENT_GROUP_DEVELOPMENT_DESCRIPTION
  "All of the tools you'll ever need to develop software")

# It doesn't make sense to install the headers without the libraries
# (because you could never use the headers!), so make the headers component
# depend on the libraries component.
set(CPACK_COMPONENT_HEADERS_DEPENDS libraries)

# Create two installation types with pre-selected components.
# The "Developer" installation has just the library and headers,
# while the "Full" installation has everything.
set(CPACK_ALL_INSTALL_TYPES Full Developer)
set(CPACK_INSTALL_TYPE_FULL_DISPLAY_NAME "Everything")
set(CPACK_COMPONENT_LIBRARIES_INSTALL_TYPES Developer Full)
set(CPACK_COMPONENT_HEADERS_INSTALL_TYPES Developer Full)
set(CPACK_COMPONENT_APPLICATIONS_INSTALL_TYPES Full)

# Include CPack to introduce the appropriate targets
include(CPack)



