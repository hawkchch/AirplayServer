cmake_minimum_required(VERSION 3.4.1)

add_subdirectory(crypto)
add_subdirectory(curve25519)
add_subdirectory(ed25519)
add_subdirectory(playfair)
add_subdirectory(plist)
add_subdirectory(${FDK_AAC_LIB_PATH} fdk_out)

include_directories(crypto curve25519 ed25519 playfair plist/plist
        ${FDK_AAC_LIB_PATH}/libAACdec/include
        ${FDK_AAC_LIB_PATH}/libAACenc/include
        ${FDK_AAC_LIB_PATH}/libFDK/include
        ${FDK_AAC_LIB_PATH}/libMpegTPDec/include
        ${FDK_AAC_LIB_PATH}/libMpegTPEnc/include
        ${FDK_AAC_LIB_PATH}/libPCMutils/include
        ${FDK_AAC_LIB_PATH}/libSBRdec/include
        ${FDK_AAC_LIB_PATH}/libSBRenc/include
        ${FDK_AAC_LIB_PATH}/libSYS/include
        #${FDK_AAC_LIB_PATH}/libArithCoding/include
        #${FDK_AAC_LIB_PATH}/libDRCdec/include
        #${FDK_AAC_LIB_PATH}/libSACdec/include
        #${FDK_AAC_LIB_PATH}/libSACenc/include
        ${MDNS_LIB_PATH}/mDNSPosix
        ${MDNS_LIB_PATH}/mDNSCore
        ${MDNS_LIB_PATH}/mDNSShared
        )

aux_source_directory(. play_src)
set(DIR_SRCS ${play_src})

if(WIN32)
    add_library( play-lib
            STATIC
            ${DIR_SRCS}
            )
else()
    add_library( play-lib
            SHARED
            ${DIR_SRCS}
            )
endif()

if(ANDROID)
    find_library( log-lib
            log )
    target_link_libraries( play-lib
            crypto
            curve25519
            ed25519
            playfair
            plist
            fdk-aac
            jdns_sd
            ${log-lib})
else()
    target_link_libraries( play-lib
            crypto
            curve25519
            ed25519
            playfair
            plist
            fdk-aac
            jdns_sd)
endif()


target_include_directories(play-lib
        PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>)