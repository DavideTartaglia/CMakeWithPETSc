cmake_minimum_required (VERSION 3.11.4)
project (poisson2D)


# The version number.
set (${PROJECT_NAME}_VERSION_MAJOR 1)
set (${PROJECT_NAME}_VERSION_MINOR 0)

# Pkg
find_package(PkgConfig REQUIRED)


# PETSc
pkg_check_modules(PETSC REQUIRED IMPORTED_TARGET PETSc)

message(STATUS "PETSC_INCLUDE_DIRS: ${PETSC_INCLUDE_DIRS}")
message(STATUS "PkgConfig::PETSC: PkgConfig::PETSC")
message(STATUS "PETSC_LIBRARIES: ${PETSC_LIBRARIES}")
message(STATUS "PETSC_CFLAGS: ${PETSC_CFLAGS}")
message(STATUS "PETSC_LDFLAGS: ${PETSC_LDFLAGS}")

# OPENMPI
find_package(MPI REQUIRED)
# add this line only when you are using openmpi which has a different c++ bindings
add_definitions(-DOMPI_SKIP_MPICXX)
message(STATUS "MPI_INCLUDE_PATH: ${MPI_INCLUDE_PATH}")


# configure a header file to pass some of the CMake settings
# to the source code
configure_file (
  "${PROJECT_SOURCE_DIR}/Config.h.in"
  "${PROJECT_BINARY_DIR}/Config.h"
)


# add the executable
add_executable (${PROJECT_NAME} poisson2D.cpp)

# add the binary tree to the search path for include files
# so that we will find Config.h
target_include_directories (${PROJECT_NAME}
PRIVATE
${PROJECT_BINARY_DIR}
${PETSC_INCLUDE_DIRS}
${MPI_INCLUDE_PATH}
)

target_compile_options(${PROJECT_NAME} PRIVATE ${PETSC_CFLAGS})

# link libraries
target_link_libraries (${PROJECT_NAME}
PRIVATE
PkgConfig::PETSC
${MPI_C_LIBRARIES}
"${PETSC_LDFLAGS}"
${EXTRA_LIBS}
)


# add the install targets
install (TARGETS ${PROJECT_NAME} DESTINATION bin)
install (FILES "${PROJECT_BINARY_DIR}/Config.h" DESTINATION include)

