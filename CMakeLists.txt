cmake_minimum_required(VERSION "2.8.0")

list(APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/modules)

project(gst-plugins-vision)

include(MacroLogFeature)

find_package(GStreamer REQUIRED COMPONENTS base)
macro_log_feature(GSTREAMER_FOUND "GStreamer" "Required to build gst-plugins-vision" "http://gstreamer.freedesktop.org/" TRUE "1.2.0")
macro_log_feature(GSTREAMER_BASE_LIBRARY_FOUND "GStreamer base library" "Required to build most plugins" "http://gstreamer.freedesktop.org/" FALSE "1.2.0")

find_package(GStreamerPluginsBase COMPONENTS video)
macro_log_feature(GSTREAMER_VIDEO_LIBRARY_FOUND "GStreamer video library" "Required to build several video plugins" "http://gstreamer.freedesktop.org/" FALSE "1.2.0")

find_package(GLIB2 REQUIRED)
macro_log_feature(GLIB2_FOUND "GLib" "Required to build gst-plugins-vision" "http://www.gtk.org/" TRUE)

find_package(GObject REQUIRED)
macro_log_feature(GOBJECT_FOUND "GObject" "Required to build gst-plugins-vision" "http://www.gtk.org/" TRUE)

find_package(NIIMAQ)
macro_log_feature(NIIMAQ_FOUND "NI-IMAQ" "Required to build National Instruments IMAQ source element" "http://www.ni.com/" FALSE)

find_package(NIIMAQDX)
macro_log_feature(NIIMAQDX_FOUND "NI-IMAQdx" "Required to build National Instruments IMAQdx source element" "http://www.ni.com/" FALSE)

find_package(FreeImage)
macro_log_feature(FREEIMAGE_FOUND "FreeImage" "Required to build FreeImage plugin" "http://freeimage.sourceforge.net/" FALSE)

find_package(OpenCV)
macro_log_feature(OPENCV_FOUND "OpenCV" "Required to build sensorfx plugin" "http://opencv.willowgarage.com/" FALSE)

find_package(EDT)
macro_log_feature(EDT_FOUND "EDT" "Required to build EDT PDV source element" "http://www.edt.com/" FALSE)

find_package(Euresys)
macro_log_feature(EURESYS_FOUND "Euresys" "Required to build Euresys source element" "http://www.euresys.com/" FALSE)

find_package(IOtechDaqX)
macro_log_feature(IOTECHDAQX_FOUND "IOtech DaqX" "Required to build IOtech DaqX source element" "http://www.euresys.com/" FALSE)

find_package(Phoenix)
macro_log_feature(PHOENIX_FOUND "Active Silicon Phoenix" "Required to build Active Silicon Phoenix source element" "http://www.activesilicon.com/" FALSE)

find_package(Sapera)
macro_log_feature(SAPERA_FOUND "Teledyne DALSA Sapera" "Required to build Teledyne DALSA Sapera source element" "http://www.teledynedalsa.com/" FALSE)

find_package(XCLIB)
macro_log_feature(XCLIB_FOUND "EPIX PIXCI" "Required to build EPIX PIXCI source element" "http://www.epixinc.com/" FALSE)


# Setup common environment
include_directories(
    .
    ${GSTREAMER_INCLUDE_DIR}
    ${GLIB2_INCLUDE_DIR})

if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    file(TO_CMAKE_PATH "${GSTREAMER_INCLUDE_DIR}/../.." PREFIX)
    set(CMAKE_INSTALL_PREFIX
        ${PREFIX} CACHE PATH "Directory to install plugins, under lib/gstreamer-0.10" FORCE)
endif(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)

# Build the plugins
add_subdirectory(ext)
add_subdirectory(gst)
add_subdirectory(sys)

macro_display_feature_log()
