cmake_minimum_required(VERSION 2.8)
include(CMakeDependentOption)

#if (NOT APPLE AND NOT WIN32)
#message(FATAL_ERROR
#"This dependency pack is intended for use on Apple OS X and MS Windows only. "
#"For other systems, such as GNU/Linux, please consult your package manager or "
#"distribution maintainer to discover how to install the libraries you wish to "
#"use with CEGUI.  Thanks!")
#endif()

SET(CMAKE_MODULE_PATH
"${CMAKE_SOURCE_DIR};${CMAKE_MODULE_PATH}")

if (APPLE)
    set( CMAKE_OSX_ARCHITECTURES x86_64;i386 CACHE STRING "Build architectures for Mac OS X" )
    set( CMAKE_BUILD_TYPE Release CACHE STRING "Sets the configuration to build (Release, Debug, etc...)")
    set( CMAKE_OSX_DEPLOYMENT_TARGET 10.7 CACHE STRING "Sets the configuration to build (Release, Debug, etc...)")
    INCLUDE_DIRECTORIES(/usr/include/malloc)
endif()

project(CEGUI-DEPS)

cmake_dependent_option( CEGUI_MSVC_STATIC_RUNTIME "Specifies whether to the static runtime (/MT and /MTd) or the DLL runtime (/MD and /MDd)." FALSE "MSVC" FALSE )
mark_as_advanced(CEGUI_MSVC_STATIC_RUNTIME)

if (APPLE)
    set(CEGUI_APPLE_DYLIB_INSTALL_PATH "@executable_path/../lib" CACHE STRING "Specifies the install path for generated dylibs." )
    mark_as_advanced(CEGUI_APPLE_DYLIB_INSTALL_PATH)
endif()

if (CEGUI_MSVC_STATIC_RUNTIME)
    foreach(_BUILD_CONFIG DEBUG RELEASE RELWITHDEBINFO MINSIZEREL)
        foreach(_BUILD_VARS CMAKE_CXX_FLAGS_${_BUILD_CONFIG} CMAKE_C_FLAGS_${_BUILD_CONFIG})
            string(REGEX REPLACE /MD /MT ${_BUILD_VARS} ${${_BUILD_VARS}})
        endforeach()
    endforeach()
endif()

if (MSVC)
    add_definitions(-D_CRT_SECURE_NO_WARNINGS)
    set(CMAKE_DEBUG_POSTFIX _d)
endif()


macro(cegui_add_library _NAME _SOURCES_VAR _HEADERS_VAR)
    add_library(${_NAME} SHARED ${${_SOURCES_VAR}} ${${_HEADERS_VAR}})

    set_target_properties(${_NAME} PROPERTIES
        ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"
        ARCHIVE_OUTPUT_DIRECTORY_DEBUG "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"
        ARCHIVE_OUTPUT_DIRECTORY_RELEASE "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"
        ARCHIVE_OUTPUT_DIRECTORY_MINSIZEREL "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"
        ARCHIVE_OUTPUT_DIRECTORY_RELWITHDEBINFO "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"

        LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"
        LIBRARY_OUTPUT_DIRECTORY_DEBUG "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"
        LIBRARY_OUTPUT_DIRECTORY_RELEASE "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"
        LIBRARY_OUTPUT_DIRECTORY_MINSIZEREL "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"
        LIBRARY_OUTPUT_DIRECTORY_RELWITHDEBINFO "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"

        RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/dependencies/bin"
        RUNTIME_OUTPUT_DIRECTORY_DEBUG "${CMAKE_BINARY_DIR}/dependencies/bin"
        RUNTIME_OUTPUT_DIRECTORY_RELEASE "${CMAKE_BINARY_DIR}/dependencies/bin"
        RUNTIME_OUTPUT_DIRECTORY_MINSIZEREL "${CMAKE_BINARY_DIR}/dependencies/bin"
        RUNTIME_OUTPUT_DIRECTORY_RELWITHDEBINFO "${CMAKE_BINARY_DIR}/dependencies/bin")

    if (APPLE)
        set_target_properties(${_NAME} PROPERTIES
            INSTALL_NAME_DIR ${CEGUI_APPLE_DYLIB_INSTALL_PATH}
            BUILD_WITH_INSTALL_RPATH TRUE
        )
    endif()

    add_library(${_NAME}_static STATIC ${${_SOURCES_VAR}} ${${_HEADERS_VAR}})

    set_target_properties(${_NAME}_static PROPERTIES
        ARCHIVE_OUTPUT_NAME ${_NAME}
        LIBRARY_OUTPUT_NAME ${_NAME}
        RUNTIME_OUTPUT_NAME ${_NAME}

        ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/dependencies/lib/static"
        ARCHIVE_OUTPUT_DIRECTORY_DEBUG "${CMAKE_BINARY_DIR}/dependencies/lib/static"
        ARCHIVE_OUTPUT_DIRECTORY_RELEASE "${CMAKE_BINARY_DIR}/dependencies/lib/static"
        ARCHIVE_OUTPUT_DIRECTORY_MINSIZEREL "${CMAKE_BINARY_DIR}/dependencies/lib/static"
        ARCHIVE_OUTPUT_DIRECTORY_RELWITHDEBINFO "${CMAKE_BINARY_DIR}/dependencies/lib/static"

        LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/dependencies/lib/static"
        LIBRARY_OUTPUT_DIRECTORY_DEBUG "${CMAKE_BINARY_DIR}/dependencies/lib/static"
        LIBRARY_OUTPUT_DIRECTORY_RELEASE "${CMAKE_BINARY_DIR}/dependencies/lib/static"
        LIBRARY_OUTPUT_DIRECTORY_MINSIZEREL "${CMAKE_BINARY_DIR}/dependencies/lib/static"
        LIBRARY_OUTPUT_DIRECTORY_RELWITHDEBINFO "${CMAKE_BINARY_DIR}/dependencies/lib/static"

        RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/dependencies/bin"
        RUNTIME_OUTPUT_DIRECTORY_DEBUG "${CMAKE_BINARY_DIR}/dependencies/bin"
        RUNTIME_OUTPUT_DIRECTORY_RELEASE "${CMAKE_BINARY_DIR}/dependencies/bin"
        RUNTIME_OUTPUT_DIRECTORY_MINSIZEREL "${CMAKE_BINARY_DIR}/dependencies/bin"
        RUNTIME_OUTPUT_DIRECTORY_RELWITHDEBINFO "${CMAKE_BINARY_DIR}/dependencies/bin")

endmacro()

macro(cegui_add_executable _NAME _SOURCES_VAR)
    add_executable(${_NAME} ${${_SOURCES_VAR}})

    set_target_properties(${_NAME} PROPERTIES
        ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"
        ARCHIVE_OUTPUT_DIRECTORY_DEBUG "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"
        ARCHIVE_OUTPUT_DIRECTORY_RELEASE "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"
        ARCHIVE_OUTPUT_DIRECTORY_MINSIZEREL "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"
        ARCHIVE_OUTPUT_DIRECTORY_RELWITHDEBINFO "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"

        LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"
        LIBRARY_OUTPUT_DIRECTORY_DEBUG "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"
        LIBRARY_OUTPUT_DIRECTORY_RELEASE "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"
        LIBRARY_OUTPUT_DIRECTORY_MINSIZEREL "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"
        LIBRARY_OUTPUT_DIRECTORY_RELWITHDEBINFO "${CMAKE_BINARY_DIR}/dependencies/lib/dynamic"

        RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/dependencies/bin"
        RUNTIME_OUTPUT_DIRECTORY_DEBUG "${CMAKE_BINARY_DIR}/dependencies/bin"
        RUNTIME_OUTPUT_DIRECTORY_RELEASE "${CMAKE_BINARY_DIR}/dependencies/bin"
        RUNTIME_OUTPUT_DIRECTORY_MINSIZEREL "${CMAKE_BINARY_DIR}/dependencies/bin"
        RUNTIME_OUTPUT_DIRECTORY_RELWITHDEBINFO "${CMAKE_BINARY_DIR}/dependencies/bin")

    #if (APPLE)
    #    set_target_properties(${_NAME} PROPERTIES
    #        INSTALL_NAME_DIR ${CEGUI_APPLE_DYLIB_INSTALL_PATH}
    #        BUILD_WITH_INSTALL_RPATH TRUE
    #    )
    #endif()

endmacro()

# only the delta3d dependencies are necessary on linux
# On linux install boost, gdal, openal, libsilly, libxerces, and libpcre as
# distro packages.
# libglm, libglfw, and libglew are not necessary for delta3d, but newer
# cegui can use them if you want the dependencies

if (APPLE OR WIN32)
cmake_dependent_option(CEGUI_BUILD_EFFECTS11
"Specifies whether to build the D3DX11Effects library.
Source code need to be located at '%DXSDK_DIR%/Samples/C++/Effects11'.
(Required for the Direct3D 11 Renderer module)" TRUE WIN32 FALSE)

option(CEGUI_BUILD_ZLIB "Specifies whether to build the zlib library
(Required for minizip resource provider support within in CEGUI).
Note: Some other dependencies may automatically enable the building of this library" FALSE)

option(CEGUI_BUILD_FREETYPE2 "Specifies whether to build the freetype 2 library.
(Required for truetype and other font support within CEGUI)" TRUE)

option(CEGUI_BUILD_PCRE "Specifies whether to build the pcre library.
(Required for text validation / restriction within CEGUI::Editbox)" TRUE)

option(CEGUI_BUILD_EXPAT "Specifies whether to build the expat XML parser library.
(Required to build the CEGUIExpatParser module for CEGUI)" FALSE)

option(CEGUI_BUILD_XERCES "Specifies whether to build the xerces-c++ XML parser library.
(Required to build the CEGUIXercesParser module for CEGUI)" TRUE)

option(CEGUI_BUILD_LUA "Specifies whether to build the lua library.
(Required to build the lua scripting module for CEGUI)" TRUE)

option(CEGUI_BUILD_SILLY "Specifies whether to build the SILLY image loading library.
Enabling this will cause other libs to be built: zlib, libpng, jpeg
(Required to build the CEGUISILLYImageCodec module for CEGUI)" TRUE)

option(CEGUI_BUILD_FREEIMAGE "Specifies whether to build the FreeImage library.
(Required to build the CEGUIFreeImageImageCodec module for CEGUI)" FALSE)

option(CEGUI_BUILD_CORONA "Specifies whether to build the Corona library.
Enabling this will cause other libs to be built: zlib, libpng, jpeg
(Required to build the CEGUICoronaImageCodec module for CEGUI)" FALSE)

option(CEGUI_BUILD_DEVIL "Specifies whether to build the IL and ILU libraries.
Enabling this will cause other libs to be built: zlib, libpng, jpeg, libmng, libtiff
(Required to build the CEGUIDevILImageCodec module for CEGUI)" FALSE)

option(CEGUI_BUILD_GLEW "Specifies whether to build the GLEW library.
(Required to build the OpenGL renderer module for CEGUI)" TRUE)

option(CEGUI_BUILD_TINYXML "Specifies whether to build the tinyxml library.
(Required to build the tinyxml based XML parser module for CEGUI)" FALSE)

option(CEGUI_BUILD_MINIZIP "Specifies whether to build the minizip library.
(Required to build the minizip based resource provider for CEGUI)" FALSE)

option(CEGUI_BUILD_GLFW "Specifies whether to build the GLFW library.
(Required to build the samples framework with OpenGL support.)" TRUE)

option(CEGUI_BUILD_GLM "Specifies whether to copy the GLM library headers.
(Required to build the OpenGL renderer module for CEGUI)" TRUE)

option(CEGUI_BUILD_TOLUAPP "Specifies whether to build the tolua++ library.
(Required to build the Lua script module for CEGUI)" TRUE)

endif()

option(DELTA3D_BUILD_ODE "Specifies whether to build the ode library.  DELTA3D" FALSE)
option(DELTA3D_BUILD_CPPUNIT "Specifies whether to build the cppunit library.  DELTA3D" TRUE)
option(DELTA3D_BUILD_NL "Specifies whether to build the Hawk NL library.  DELTA3D" TRUE)
option(DELTA3D_BUILD_GNE "Specifies whether to build the GNE library.  DELTA3D" TRUE)
option(DELTA3D_BUILD_CAL3D "Specifies whether to build the cal3d library.  DELTA3D" TRUE)
option(DELTA3D_BUILD_ALUT "Specifies whether to build the alut library. DELTA3D" TRUE)
option(DELTA3D_USE_EXTERNAL_OPENEXR "Specifies whether to assume an external openexr dependency. DELTA3D" FALSE)
cmake_dependent_option(DELTA3D_BUILD_OPENEXR "Specifies whether to build the openexr library. DELTA3D" ON "NOT DELTA3D_USE_EXTERNAL_OPENEXR"  OFF)
option(DELTA3D_BUILD_OPENVDB "Specifies whether to build the openvdb library. DELTA3D" TRUE)

if (CEGUI_BUILD_SILLY OR CEGUI_BUILD_CORONA OR CEGUI_BUILD_DEVIL)
    set(CEGUI_BUILD_LIBPNG TRUE)
    set(CEGUI_BUILD_JPEG TRUE)
endif()

if (CEGUI_BUILD_DEVIL)
	set(CEGUI_BUILD_LIBMNG TRUE)
	set(CEGUI_BUILD_LIBTIFF TRUE)
endif()

if (CEGUI_BUILD_PCRE OR CEGUI_BUILD_LIBPNG OR CEGUI_BUILD_LIBTIFF OR CEGUI_BUILD_MINIZIP OR DELTA3D_BUILD_OPENEXR)
    set(CEGUI_BUILD_ZLIB TRUE)
endif()

if (CEGUI_BUILD_TOLUAPP)
	set(CEGUI_BUILD_LUA TRUE)
endif()

file(MAKE_DIRECTORY "${CMAKE_BINARY_DIR}/dependencies/include")

if (WIN32 AND CEGUI_BUILD_EFFECTS11 AND EXISTS "$ENV{DXSDK_DIR}/Samples/C++/Effects11")
    add_subdirectory(src/D3D11Effects/CEGUI-BUILD)
    set(Effects11Dep D3DX11Effects)
endif()

if (CEGUI_BUILD_ZLIB)
    add_subdirectory(src/zlib-1.2.8/CEGUI-BUILD)
    set(zlibDep zlib)
endif()

if (CEGUI_BUILD_LIBPNG)
    add_subdirectory(src/libpng-1.4.7/CEGUI-BUILD)
    set(libpngDep libpng)
endif()

if (CEGUI_BUILD_JPEG)
    add_subdirectory(src/jpeg-8c/CEGUI-BUILD)
    set(jpegDep jpeg)
endif()

if (CEGUI_BUILD_LIBMNG)
    add_subdirectory(src/libmng-1.0.10/CEGUI-BUILD)
    set(libmngDep libmng)
endif()

if (CEGUI_BUILD_LIBTIFF)
    add_subdirectory(src/tiff-3.9.5/CEGUI-BUILD)
    set(libtiffDep libtiff)
endif()

if (CEGUI_BUILD_FREETYPE2)
    add_subdirectory(src/freetype-2.4.4/CEGUI-BUILD)
    set(freetypeDep freetype)
endif()

if (CEGUI_BUILD_PCRE)
    add_subdirectory(src/pcre-8.36/CEGUI-BUILD)
    set(pcreDep pcre)
endif()

if (CEGUI_BUILD_LUA)
    add_subdirectory(src/lua-5.1.4/CEGUI-BUILD)
    set(luaDep lua)
endif()

if (CEGUI_BUILD_EXPAT)
    add_subdirectory(src/expat-2.0.1/CEGUI-BUILD)
    set(expatDep expat)
endif()

if (CEGUI_BUILD_XERCES)
    add_subdirectory(src/xerces-c-3.1.2/CEGUI-BUILD)
    set(xercesDep xerces-c_3)
endif()

if (CEGUI_BUILD_SILLY)
    add_subdirectory(src/SILLY/CEGUI-BUILD)
    set(SILLYDep SILLY)
endif()

if (CEGUI_BUILD_FREEIMAGE)
    add_subdirectory(src/FreeImage-3.15.0/CEGUI-BUILD)
    set(FreeImageDep FreeImage)
endif()

if (CEGUI_BUILD_CORONA)
    add_subdirectory(src/corona-1.0.2/CEGUI-BUILD)
    set(coronaDep corona)
endif()

if (CEGUI_BUILD_DEVIL)
    add_subdirectory(src/devil-1.7.8/CEGUI-BUILD)
    set(ILDep IL)
    set(ILUDep ILU)
endif()

if (CEGUI_BUILD_GLEW)
    add_subdirectory(src/glew-1.7.0/CEGUI-BUILD)
    set(glewDep glew)
endif()

if (CEGUI_BUILD_TINYXML)
    add_subdirectory(src/tinyxml-2.6.2/CEGUI-BUILD)
    set(tinyxmlDep tinyxml)
endif()

if (CEGUI_BUILD_MINIZIP)
    add_subdirectory(src/zlib-1.2.5/contrib/minizip/CEGUI-BUILD)
    set(minizipDep minizip)
endif()

if (CEGUI_BUILD_GLFW)
    add_subdirectory(src/glfw-2.7.2/CEGUI-BUILD)
    set(glfwDep glfw)
endif()

if (CEGUI_BUILD_GLM)
    add_subdirectory(src/glm-0.9.3.1/CEGUI-BUILD)
endif()

if (CEGUI_BUILD_TOLUAPP)
    add_subdirectory(src/tolua++-1.0.93/CEGUI-BUILD)
    set(toluappDep toluapp)
endif()

if (DELTA3D_BUILD_ODE)
    add_subdirectory(src/ode-0.13.1/CEGUI-BUILD)
endif()

if (DELTA3D_BUILD_CPPUNIT)
    add_subdirectory(src/cppunit-1.13.1/CEGUI-BUILD)
endif()

if (DELTA3D_BUILD_NL)
    add_subdirectory(src/HawkNL1.68/CEGUI-BUILD)
endif()

if (DELTA3D_BUILD_GNE)
    add_subdirectory(src/gnelib-0_75-trunk/CEGUI-BUILD)
endif()

if (DELTA3D_BUILD_CAL3D)    
   add_subdirectory(src/cal3d-trunk/CEGUI-BUILD)
endif()

if (DELTA3D_BUILD_ALUT)    
   add_subdirectory(src/freealut-1.1.0-src)
endif()

if (DELTA3D_BUILD_OPENEXR OR (DELTA3D_BUILD_OPENVDB AND NOT DELTA3D_USE_EXTERNAL_OPENEXR))    
   add_subdirectory(src/ilmbase-2.2.0)
   add_subdirectory(src/openexr-2.2.0)
endif()

if (DELTA3D_BUILD_OPENVDB) 
   add_subdirectory(src/openvdb/CEGUI-BUILD)
endif()

add_custom_target(POST_BUILD_CLEANUP ALL DEPENDS
    ${zlibDep} ${libpngDep} ${jpegDep} ${libmngDep} ${libtiffDep} ${freetypeDep} ${pcreDep} ${luaDep} ${expatDep} ${xercesDep} ${SILLYDep} ${FreeImageDep} ${coronaDep} ${ILDep} ${ILUDep} ${Effects11Dep}
    ${glewDep} ${tinyxmlDep} ${minizipDep} ${toluappDep} ${glfwDep})

if (MSVC)
    add_custom_command(TARGET POST_BUILD_CLEANUP COMMAND ${CMAKE_COMMAND} -P "${CMAKE_SOURCE_DIR}/msvc_postbuild.cmake")
endif()

