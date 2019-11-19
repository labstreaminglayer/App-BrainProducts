cmake_minimum_required(VERSION 3.5)
project(LiveAmp
	LANGUAGES CXX
	VERSION 0.1.0)
list(APPEND LSL_INSTALL_ROOT "${CMAKE_CURRENT_LIST_DIR}/../../../build/install")
include(Findliblsl.cmake)

# THIRD PARTY LIBRARIES #
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)
find_package(Qt5 COMPONENTS Xml Widgets REQUIRED)
find_package(Boost REQUIRED COMPONENTS thread)

# liveamplib2
set(LiveAmpLib2 ${CMAKE_CURRENT_LIST_DIR}/LiveAmpLib2.lib)

# Target name
set(target LiveAmp)

# Sources
set(sources
	main.cpp
	mainwindow.cpp
	mainwindow.h
	mainwindow.ui
	LiveAmp.cpp
	LiveAmp.h
	Amplifier_LIB.h
)

# Add executable target
add_executable(${target} MACOSX_BUNDLE WIN32 ${sources})

target_link_libraries(${target}
	PRIVATE
	Qt5::Widgets
	Qt5::Xml
	Boost::thread
	LSL::lsl
	${LiveAmpLib2}
)

# Boost includes needed here because they aren't pulled in by any linked library above
target_include_directories(${target} PRIVATE ${Boost_INCLUDE_DIRS})

# Deployment
installLSLApp(${target})
install(FILES default_config.cfg
	DESTINATION ${PROJECT_NAME})
	
installLSLAuxFiles(${target} LiveAmpLib2.dll)
