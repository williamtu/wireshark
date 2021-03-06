# CMakeLists.txt
#
# Wireshark - Network traffic analyzer
# By Gerald Combs <gerald@wireshark.org>
# Copyright 1998 Gerald Combs
#
# This program is free software; you can redistribute it and/or
# modify it under the terms of the GNU General Public License
# as published by the Free Software Foundation; either version 2
# of the License, or (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.
#

ADD_CUSTOM_CMAKE_INCLUDE()

include_directories(
	${CMAKE_CURRENT_BINARY_DIR}
	${CMAKE_CURRENT_SOURCE_DIR}
)

set(CRYPT_FILES
	airpdcap.c
	airpdcap_ccmp.c
	airpdcap_debug.c
	airpdcap_rijndael.c
	airpdcap_tkip.c
	${CUSTOM_CRYPT_SRC}
)
source_group(crypt FILES ${CRYPT_FILES})

if(WERROR_COMMON_FLAGS)
	set_source_files_properties(
		${CRYPT_FILES}
		PROPERTIES
		COMPILE_FLAGS ${WERROR_COMMON_FLAGS}
	)
endif()

add_library(crypt OBJECT
	${CRYPT_FILES}
)

set_target_properties(crypt PROPERTIES
	FOLDER "Libs/epan/crypt"
	COMPILE_DEFINITIONS "WS_BUILD_DLL"
)

CHECKAPI(
	NAME
	  crypt
	SWITCHES
	  -g termoutput -build
	SOURCES
	  ${CRYPT_FILES}
)

#
# Editor modelines  -  http://www.wireshark.org/tools/modelines.html
#
# Local variables:
# c-basic-offset: 8
# tab-width: 8
# indent-tabs-mode: t
# End:
#
# vi: set shiftwidth=8 tabstop=8 noexpandtab:
# :indentSize=8:tabSize=8:noTabs=false:
#
