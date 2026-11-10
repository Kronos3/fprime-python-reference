####
# This sets up the build system for the 'FprimePythonReference' project, including
# components and deployments from project.cmake. In addition, it imports the core F Prime components.
####

cmake_minimum_required(VERSION 3.24.2)
project(FprimePythonReference C CXX)


###
# F' Core Setup
# This includes all of the F prime core components, and imports the make-system.
###
include("${CMAKE_CURRENT_LIST_DIR}/lib/fprime/cmake/FPrime.cmake")

# Set up the Python bindings: pybind11, position independent code, and the fprime_python build target
include(FprimePython)


# NOTE: register custom targets between these two lines
fprime_setup_included_code()


# This includes project-wide objects
add_fprime_subdirectory("${CMAKE_CURRENT_LIST_DIR}/FprimePythonReference")
