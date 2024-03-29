# Copyright (C) 2018 Fondazione Istituto Italiano di Tecnologia (IIT)
# All Rights Reserved.
# Authors: Elisa Maiettini <elisa.maiettini@iit.it>


cmake_minimum_required(VERSION 3.1)
project(mutual-gaze-classifier-demo)

set(import_path "${CMAKE_CURRENT_SOURCE_DIR}/functions")


if(NOT EXISTS ${import_path})
	message(FATAL_ERROR "the python functions in utilitis and config are not in the expected folder.")
endif()

configure_file(mutualgaze-classifier.py.in mutualgaze-classifier.py @ONLY)
configure_file(multiface-mutualgaze-classifier.py.in multiface-mutualgaze-classifier.py @ONLY)

set(python_bin ${CMAKE_CURRENT_SOURCE_DIR}/../build/src/mutualgaze-classifier.py
			   ${CMAKE_CURRENT_SOURCE_DIR}/../build/src/multiface-mutualgaze-classifier.py)

add_custom_target(copy_python_in_build ALL)
add_custom_command(TARGET copy_python_in_build POST_BUILD
	               COMMAND ${CMAKE_COMMAND} -E copy ${python_bin} ${CMAKE_BINARY_DIR}/${CMAKE_CFG_INTDIR}
	               COMMENT "Copying ${python_bin} to ${CMAKE_BINARY_DIR}/${CMAKE_CFG_INTDIR}/")

install(PROGRAMS ${python_bin} DESTINATION bin)

