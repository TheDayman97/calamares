find_package(ECM 5.10.0 NO_MODULE)
if( ECM_FOUND )
    set(CMAKE_MODULE_PATH ${ECM_MODULE_PATH} ${CMAKE_MODULE_PATH})
    include( ECMAddTests )
endif()

find_package( Qt5 COMPONENTS Core Test REQUIRED )

list( APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/CMakeModules" )
find_package( Crypt )

include_directories( ${PROJECT_BINARY_DIR}/src/libcalamaresui )

calamares_add_plugin( users
    TYPE viewmodule
    EXPORT_MACRO PLUGINDLLEXPORT_PRO
    SOURCES
        CreateUserJob.cpp
        SetPasswordJob.cpp
        UsersViewStep.cpp
        UsersPage.cpp
        SetHostNameJob.cpp
    UI
        page_usersetup.ui
    RESOURCES
        users.qrc
    LINK_PRIVATE_LIBRARIES
        calamaresui
        ${CRYPT_LIBRARIES}
    SHARED_LIB
)

if( ECM_FOUND )
    ecm_add_test(
            PasswordTests.cpp
            SetPasswordJob.cpp
        TEST_NAME
            passwordtest
        LINK_LIBRARIES
            ${CALAMARES_LIBRARIES}
            Qt5::Core
            Qt5::Test
            ${CRYPT_LIBRARIES}
    )
    set_target_properties( passwordtest PROPERTIES AUTOMOC TRUE )
endif()
