cmake_minimum_required(VERSION 3.0)

# Create main project.
project(yaml-fortran VERSION 0.1.0 LANGUAGES Fortran)

set(GIT_COMMIT_ID ${yaml-fortran_VERSION})
set(GIT_BRANCH_NAME unknown)
configure_file(yaml_version.F90.in yaml_version.F90)

add_library(yaml-fortran
            ${CMAKE_CURRENT_BINARY_DIR}/yaml_version.F90
            yaml_types.F90
            yaml_settings.F90
            yaml.F90
)
set_property(TARGET yaml-fortran PROPERTY Fortran_MODULE_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/modules")
target_include_directories(yaml-fortran
   PUBLIC
      $<INSTALL_INTERFACE:include>
      $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/modules>
)

if(YAML_FORTRAN_BUILD_TEST)
add_executable(test_yaml EXCLUDE_FROM_ALL
               test_yaml.F90
              )
target_link_libraries(test_yaml PRIVATE yaml)
set_property(TARGET test_yaml PROPERTY FOLDER yaml)
endif()

set_property(TARGET yaml-fortran PROPERTY FOLDER yaml)

install(TARGETS yaml-fortran
        EXPORT yamlConfig
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
)

install(EXPORT yamlConfig DESTINATION cmake)
