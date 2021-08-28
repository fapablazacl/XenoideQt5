set (target "XenoideQt")

set (CMAKE_INCLUDE_CURRENT_DIR ON)
set (CMAKE_AUTOMOC ON)
set (CMAKE_AUTOUIC ON)

find_package(Qt5 COMPONENTS Core Widgets REQUIRED)

# this doesn't work on Linux (arch)
if (WIN32)
  find_library(QScintilla REQUIRED)
endif()

set (sources
    "FindReplaceDialogQt.hpp"
    "IDEFrameQt.hpp"
    "DialogManagerQt.hpp"
    "FolderBrowserQt.hpp"
    "FileSearchDialogQt.hpp"
    "FileSearchDialogItemWidgetQt.hpp"
    "UtilitiesQt.hpp"
    "DocumentManagerMdiQt.hpp"
    "DocumentMdiSubWindowQt.hpp"

    "main.cpp"
    "FindReplaceDialogQt.cpp"
    "IDEFrameQt.cpp"
    "DialogManagerQt.cpp"
    "FolderBrowserQt.cpp"
    "FileSearchDialogQt.cpp"
    "FileSearchDialogItemWidgetQt.cpp"
    "UtilitiesQt.cpp"
    "DocumentManagerMdiQt.cpp"
    "DocumentMdiSubWindowQt.cpp"
)

add_definitions (-DXENOIDE_TARGET_NAME=${target})
add_library(${target} ${sources})

# this is needed for QScintilla
target_include_directories(${target} PRIVATE "${Qt5_DIR}/../../../include")
target_link_directories(${target} PUBLIC "${Qt5_DIR}/../../")

target_include_directories(${target} PUBLIC "include/")
target_compile_definitions (${target} PRIVATE "QSCINTILLA_DLL")

if (WIN32)
    target_link_libraries (
        ${target} 
        Qt5::Widgets
        debug qscintilla2_qt5d
        optimized qscintilla2_qt5
        Xenoide.Core
        Xenoide.Gui
    )
else()
    target_link_libraries (
        ${target} 
        Qt5::Widgets
        qscintilla2_qt5
        Xenoide.Core
        Xenoide.Gui
    )
endif()

install (TARGETS ${target} DESTINATION "lib")
