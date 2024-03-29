cmake_minimum_required(VERSION 3.2)

set(TRAVIS_BUILD_NUMBER $ENV{TRAVIS_BUILD_NUMBER})
if(NOT TRAVIS_BUILD_NUMBER)
    set(TRAVIS_BUILD_NUMBER 1)
endif()

project(ip_filter VERSION 0.0.${TRAVIS_BUILD_NUMBER})

if (MSVC)
    set(COMPILE_OPTS /W4)
else ()
    set(COMPILE_OPTS -Wall -Wextra -pedantic -Werror)
endif()

add_executable(ip_filter src/ip_filter.cpp)

set(timus_executables)
foreach(task_number
        1000
        1001
        1293
        1785
        2012)
    add_executable(timus_${task_number} timus/${task_number}.cpp)
    target_compile_options(timus_${task_number} PRIVATE ${COMPILE_OPTS})
    list(APPEND timus_executables timus_${task_number})
endforeach(task_number)

set_target_properties(ip_filter ${timus_executables} PROPERTIES
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
)

target_compile_options(ip_filter PRIVATE ${COMPILE_OPTS})

install(TARGETS ip_filter RUNTIME DESTINATION bin)

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_PACKAGE_CONTACT mike.42.alexeev@yandex.ru)

include(CPack)
