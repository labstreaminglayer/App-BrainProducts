cmake_minimum_required(VERSION 3.5)
project(BrainVisionRDA
	LANGUAGES CXX
	VERSION 0.1.0)
list(APPEND LSL_INSTALL_ROOT "${CMAKE_CURRENT_LIST_DIR}/../../../build/install")
include(Findliblsl.cmake)

# META CONFIG #
set(META_PROJECT_DESCRIPTION "Stream from BrainVision Recorder's RDA client via Lab Streaming Layer")

# THIRD PARTY LIBRARIES #
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)
find_package(Qt5 COMPONENTS Xml Widgets REQUIRED)
find_package(Boost REQUIRED COMPONENTS thread)

# Target name
set(target BrainVisionRDA)

# Sources
set(sources
	main.cpp
	mainwindow.cpp
	mainwindow.h
	mainwindow.ui
	rda_client.h
	rda_message.h
	regression.h
)

# Add executable target
add_executable(${target} MACOSX_BUNDLE WIN32 ${sources})

target_link_libraries(${target}
	PRIVATE
	Qt5::Widgets
	Qt5::Xml
    Qt5::Gui
	Boost::thread
	LSL::lsl
)

# Boost includes needed here because they aren't pulled in by any linked library above
target_include_directories(${target} PRIVATE ${Boost_INCLUDE_DIRS})

# Deployment
installLSLApp(${target})
install(FILES default_config.cfg
	DESTINATION ${PROJECT_NAME})
	
installLSLAuxFiles(${target} Qt5Core.dll Qt5Gui.dll Qt5Widgets.dll)
