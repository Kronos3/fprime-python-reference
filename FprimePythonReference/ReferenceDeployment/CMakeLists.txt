#####
# 'ReferenceDeployment' Deployment:
#
# This registers the 'ReferenceDeployment' deployment to the build system. 
# Custom components that have not been added at the project-level should be added to 
# the list below.
#
#####

add_fprime_subdirectory("${CMAKE_CURRENT_LIST_DIR}/Top/")

register_fprime_deployment(
    SOURCES
        "${CMAKE_CURRENT_LIST_DIR}/Main.cpp" # Needed to appease compiler
        "${CMAKE_CURRENT_LIST_DIR}/fsw_main.py" # Python entrypoint
    CHOOSES_IMPLEMENTATIONS
        FprimePython_PythonConsole
    DEPENDS
        ${FPRIME_CURRENT_MODULE}_Top
)
