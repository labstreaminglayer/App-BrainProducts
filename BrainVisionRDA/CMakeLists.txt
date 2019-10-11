cmake_minimum_required(VERSION 3.5)
project(BrainVisionRDA
	LANGUAGES CXX
	VERSION 0.1.0)
include(Findliblsl.cmake)

# META CONFIG #
set(META_PROJECT_DESCRIPTION "Stream from BrainVision Recorder's RDA client via Lab Streaming Layer")

# THIRD PARTY LIBRARIES #
find_package(Qt5 COMPONENTS Xml Widgets REQUIRED)
find_package(Boost REQUIRED COMPONENTS thread)

# Add executable target
add_executable(${PROJECT_NAME} MACOSX_BUNDLE WIN32
	main.cpp
	mainwindow.cpp
	mainwindow.h
	mainwindow.ui
	rda_client.h
	rda_message.h
	regression.h
)
target_link_libraries(${PROJECT_NAME}
	PRIVATE
	Qt5::Widgets
	Qt5::Xml
    Qt5::Gui
	Boost::thread
	LSL::lsl
)

# Deployment
installLSLApp(${PROJECT_NAME})
installLSLAuxFiles(${PROJECT_NAME}
	default_config.cfg)
LSLGenerateCPackConfig()
