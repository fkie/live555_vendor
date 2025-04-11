cmake_minimum_required(VERSION 3.13...3.28)
project(
    live555_vendor
    VERSION 2024.10.24
    LANGUAGES C CXX
)

include(CMakePackageConfigHelpers)
include(GNUInstallDirs)

# Options
option(BUILD_SHARED_LIBS "Build shared libraries" ON)
option(LIVE555_ALLOW_SERVER_PORT_REUSE "Allow reuse of existing ports (not recommended)" OFF)

# Dependencies
find_package(OpenSSL QUIET)
if(TARGET OpenSSL::SSL)
    set(LIVE555_OPENSSL_SUPPORT YES)
else()
    set(LIVE555_OPENSSL_SUPPORT NO)
endif()

# Targets
add_library(
    BasicUsageEnvironment
    live555/UsageEnvironment/HashTable.cpp
    live555/UsageEnvironment/strDup.cpp
    live555/UsageEnvironment/UsageEnvironment.cpp
    live555/BasicUsageEnvironment/BasicHashTable.cpp
    live555/BasicUsageEnvironment/BasicTaskScheduler.cpp
    live555/BasicUsageEnvironment/BasicTaskScheduler0.cpp
    live555/BasicUsageEnvironment/BasicUsageEnvironment.cpp
    live555/BasicUsageEnvironment/BasicUsageEnvironment0.cpp
    live555/BasicUsageEnvironment/DelayQueue.cpp
)
target_compile_features(BasicUsageEnvironment PRIVATE cxx_std_20)
target_compile_definitions(
    BasicUsageEnvironment PRIVATE _LARGEFILE_SOURCE=1 _FILE_OFFSET_BITS=64
)
target_include_directories(
    BasicUsageEnvironment
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/live555/UsageEnvironment/include>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/live555/BasicUsageEnvironment/include>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/live555>
    PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/live555/groupsock/include
)
set_target_properties(
    BasicUsageEnvironment PROPERTIES SOVERSION ${PROJECT_VERSION}
)
add_library(live555::BasicUsageEnvironment ALIAS BasicUsageEnvironment)
add_library(
    groupsock
    live555/groupsock/GroupEId.cpp
    live555/groupsock/Groupsock.cpp
    live555/groupsock/GroupsockHelper.cpp
    live555/groupsock/inet.c
    live555/groupsock/IOHandlers.cpp
    live555/groupsock/NetAddress.cpp
    live555/groupsock/NetInterface.cpp
)
target_compile_definitions(
    groupsock PRIVATE SOCKLEN_T=socklen_t _LARGEFILE_SOURCE=1
                      _FILE_OFFSET_BITS=64
)
target_include_directories(
    groupsock
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/live555/groupsock/include>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/live555>
)
target_link_libraries(groupsock PUBLIC BasicUsageEnvironment)
set_target_properties(groupsock PROPERTIES SOVERSION ${PROJECT_VERSION})
add_library(live555::groupsock ALIAS groupsock)
add_library(
    liveMedia
    live555/liveMedia/AC3AudioFileServerMediaSubsession.cpp
    live555/liveMedia/AC3AudioRTPSink.cpp
    live555/liveMedia/AC3AudioRTPSource.cpp
    live555/liveMedia/AC3AudioStreamFramer.cpp
    live555/liveMedia/ADTSAudioFileServerMediaSubsession.cpp
    live555/liveMedia/ADTSAudioFileSource.cpp
    live555/liveMedia/ADTSAudioStreamDiscreteFramer.cpp
    live555/liveMedia/AMRAudioFileServerMediaSubsession.cpp
    live555/liveMedia/AMRAudioFileSink.cpp
    live555/liveMedia/AMRAudioFileSource.cpp
    live555/liveMedia/AMRAudioRTPSink.cpp
    live555/liveMedia/AMRAudioRTPSource.cpp
    live555/liveMedia/AMRAudioSource.cpp
    live555/liveMedia/AudioInputDevice.cpp
    live555/liveMedia/AudioRTPSink.cpp
    live555/liveMedia/AVIFileSink.cpp
    live555/liveMedia/Base64.cpp
    live555/liveMedia/BasicUDPSink.cpp
    live555/liveMedia/BasicUDPSource.cpp
    live555/liveMedia/BitVector.cpp
    live555/liveMedia/ByteStreamFileSource.cpp
    live555/liveMedia/ByteStreamMemoryBufferSource.cpp
    live555/liveMedia/ByteStreamMultiFileSource.cpp
    live555/liveMedia/COPYING
    live555/liveMedia/COPYING.LESSER
    live555/liveMedia/DeviceSource.cpp
    live555/liveMedia/DigestAuthentication.cpp
    live555/liveMedia/DVVideoFileServerMediaSubsession.cpp
    live555/liveMedia/DVVideoRTPSink.cpp
    live555/liveMedia/DVVideoRTPSource.cpp
    live555/liveMedia/DVVideoStreamFramer.cpp
    live555/liveMedia/EBMLNumber.cpp
    live555/liveMedia/EBMLNumber.hh
    live555/liveMedia/FileServerMediaSubsession.cpp
    live555/liveMedia/FileSink.cpp
    live555/liveMedia/FramedFileSource.cpp
    live555/liveMedia/FramedFilter.cpp
    live555/liveMedia/FramedSource.cpp
    live555/liveMedia/GenericMediaServer.cpp
    live555/liveMedia/grep
    live555/liveMedia/GSMAudioRTPSink.cpp
    live555/liveMedia/H261VideoRTPSource.cpp
    live555/liveMedia/H263plusVideoFileServerMediaSubsession.cpp
    live555/liveMedia/H263plusVideoRTPSink.cpp
    live555/liveMedia/H263plusVideoRTPSource.cpp
    live555/liveMedia/H263plusVideoStreamFramer.cpp
    live555/liveMedia/H263plusVideoStreamParser.cpp
    live555/liveMedia/H263plusVideoStreamParser.hh
    live555/liveMedia/H264or5VideoFileSink.cpp
    live555/liveMedia/H264or5VideoRTPSink.cpp
    live555/liveMedia/H264or5VideoStreamDiscreteFramer.cpp
    live555/liveMedia/H264or5VideoStreamFramer.cpp
    live555/liveMedia/H264VideoFileServerMediaSubsession.cpp
    live555/liveMedia/H264VideoFileSink.cpp
    live555/liveMedia/H264VideoRTPSink.cpp
    live555/liveMedia/H264VideoRTPSource.cpp
    live555/liveMedia/H264VideoStreamDiscreteFramer.cpp
    live555/liveMedia/H264VideoStreamFramer.cpp
    live555/liveMedia/H265VideoFileServerMediaSubsession.cpp
    live555/liveMedia/H265VideoFileSink.cpp
    live555/liveMedia/H265VideoRTPSink.cpp
    live555/liveMedia/H265VideoRTPSource.cpp
    live555/liveMedia/H265VideoStreamDiscreteFramer.cpp
    live555/liveMedia/H265VideoStreamFramer.cpp
    live555/liveMedia/HLSSegmenter.cpp
    live555/liveMedia/HMAC_SHA1.cpp
    live555/liveMedia/InputFile.cpp
    live555/liveMedia/JPEG2000VideoRTPSink.cpp
    live555/liveMedia/JPEG2000VideoRTPSource.cpp
    live555/liveMedia/JPEGVideoRTPSink.cpp
    live555/liveMedia/JPEGVideoRTPSource.cpp
    live555/liveMedia/JPEGVideoSource.cpp
    live555/liveMedia/Locale.cpp
    live555/liveMedia/Makefile.head
    live555/liveMedia/Makefile.tail
    live555/liveMedia/MatroskaDemuxedTrack.cpp
    live555/liveMedia/MatroskaDemuxedTrack.hh
    live555/liveMedia/MatroskaFile.cpp
    live555/liveMedia/MatroskaFileParser.cpp
    live555/liveMedia/MatroskaFileParser.hh
    live555/liveMedia/MatroskaFileServerDemux.cpp
    live555/liveMedia/MatroskaFileServerMediaSubsession.cpp
    live555/liveMedia/MatroskaFileServerMediaSubsession.hh
    live555/liveMedia/Media.cpp
    live555/liveMedia/MediaSession.cpp
    live555/liveMedia/MediaSink.cpp
    live555/liveMedia/MediaSource.cpp
    live555/liveMedia/MIKEY.cpp
    live555/liveMedia/MP3ADU.cpp
    live555/liveMedia/MP3ADUdescriptor.cpp
    live555/liveMedia/MP3ADUdescriptor.hh
    live555/liveMedia/MP3ADUinterleaving.cpp
    live555/liveMedia/MP3ADURTPSink.cpp
    live555/liveMedia/MP3ADURTPSource.cpp
    live555/liveMedia/MP3ADUTranscoder.cpp
    live555/liveMedia/MP3AudioFileServerMediaSubsession.cpp
    live555/liveMedia/MP3AudioMatroskaFileServerMediaSubsession.cpp
    live555/liveMedia/MP3AudioMatroskaFileServerMediaSubsession.hh
    live555/liveMedia/MP3FileSource.cpp
    live555/liveMedia/MP3Internals.cpp
    live555/liveMedia/MP3Internals.hh
    live555/liveMedia/MP3InternalsHuffman.cpp
    live555/liveMedia/MP3InternalsHuffman.hh
    live555/liveMedia/MP3InternalsHuffmanTable.cpp
    live555/liveMedia/MP3StreamState.cpp
    live555/liveMedia/MP3StreamState.hh
    live555/liveMedia/MP3Transcoder.cpp
    live555/liveMedia/MPEG1or2AudioRTPSink.cpp
    live555/liveMedia/MPEG1or2AudioRTPSource.cpp
    live555/liveMedia/MPEG1or2AudioStreamFramer.cpp
    live555/liveMedia/MPEG1or2Demux.cpp
    live555/liveMedia/MPEG1or2DemuxedElementaryStream.cpp
    live555/liveMedia/MPEG1or2DemuxedServerMediaSubsession.cpp
    live555/liveMedia/MPEG1or2FileServerDemux.cpp
    live555/liveMedia/MPEG1or2VideoFileServerMediaSubsession.cpp
    live555/liveMedia/MPEG1or2VideoRTPSink.cpp
    live555/liveMedia/MPEG1or2VideoRTPSource.cpp
    live555/liveMedia/MPEG1or2VideoStreamDiscreteFramer.cpp
    live555/liveMedia/MPEG1or2VideoStreamFramer.cpp
    live555/liveMedia/MPEG2IndexFromTransportStream.cpp
    live555/liveMedia/MPEG2TransportFileServerMediaSubsession.cpp
    live555/liveMedia/MPEG2TransportStreamAccumulator.cpp
    live555/liveMedia/MPEG2TransportStreamDemux.cpp
    live555/liveMedia/MPEG2TransportStreamDemuxedTrack.cpp
    live555/liveMedia/MPEG2TransportStreamDemuxedTrack.hh
    live555/liveMedia/MPEG2TransportStreamFramer.cpp
    live555/liveMedia/MPEG2TransportStreamFromESSource.cpp
    live555/liveMedia/MPEG2TransportStreamFromPESSource.cpp
    live555/liveMedia/MPEG2TransportStreamIndexFile.cpp
    live555/liveMedia/MPEG2TransportStreamMultiplexor.cpp
    live555/liveMedia/MPEG2TransportStreamParser_PAT.cpp
    live555/liveMedia/MPEG2TransportStreamParser_PMT.cpp
    live555/liveMedia/MPEG2TransportStreamParser_STREAM.cpp
    live555/liveMedia/MPEG2TransportStreamParser.cpp
    live555/liveMedia/MPEG2TransportStreamParser.hh
    live555/liveMedia/MPEG2TransportStreamTrickModeFilter.cpp
    live555/liveMedia/MPEG2TransportUDPServerMediaSubsession.cpp
    live555/liveMedia/MPEG4ESVideoRTPSink.cpp
    live555/liveMedia/MPEG4ESVideoRTPSource.cpp
    live555/liveMedia/MPEG4GenericRTPSink.cpp
    live555/liveMedia/MPEG4GenericRTPSource.cpp
    live555/liveMedia/MPEG4LATMAudioRTPSink.cpp
    live555/liveMedia/MPEG4LATMAudioRTPSource.cpp
    live555/liveMedia/MPEG4VideoFileServerMediaSubsession.cpp
    live555/liveMedia/MPEG4VideoStreamDiscreteFramer.cpp
    live555/liveMedia/MPEG4VideoStreamFramer.cpp
    live555/liveMedia/MPEGVideoStreamFramer.cpp
    live555/liveMedia/MPEGVideoStreamParser.cpp
    live555/liveMedia/MPEGVideoStreamParser.hh
    live555/liveMedia/MultiFramedRTPSink.cpp
    live555/liveMedia/MultiFramedRTPSource.cpp
    live555/liveMedia/OggDemuxedTrack.cpp
    live555/liveMedia/OggDemuxedTrack.hh
    live555/liveMedia/OggFile.cpp
    live555/liveMedia/OggFileParser.cpp
    live555/liveMedia/OggFileParser.hh
    live555/liveMedia/OggFileServerDemux.cpp
    live555/liveMedia/OggFileServerMediaSubsession.cpp
    live555/liveMedia/OggFileServerMediaSubsession.hh
    live555/liveMedia/OggFileSink.cpp
    live555/liveMedia/OnDemandServerMediaSubsession.cpp
    live555/liveMedia/ourMD5.cpp
    live555/liveMedia/OutputFile.cpp
    live555/liveMedia/PassiveServerMediaSubsession.cpp
    live555/liveMedia/ProxyServerMediaSession.cpp
    live555/liveMedia/QCELPAudioRTPSource.cpp
    live555/liveMedia/QuickTimeFileSink.cpp
    live555/liveMedia/QuickTimeGenericRTPSource.cpp
    live555/liveMedia/RawVideoRTPSink.cpp
    live555/liveMedia/RawVideoRTPSource.cpp
    live555/liveMedia/rtcp_from_spec.c
    live555/liveMedia/rtcp_from_spec.h
    live555/liveMedia/RTCP.cpp
    live555/liveMedia/RTPInterface.cpp
    live555/liveMedia/RTPSink.cpp
    live555/liveMedia/RTPSource.cpp
    live555/liveMedia/RTSPClient.cpp
    live555/liveMedia/RTSPCommon.cpp
    live555/liveMedia/RTSPCommon.cpp.new
    live555/liveMedia/RTSPRegisterSender.cpp
    live555/liveMedia/RTSPServer.cpp
    live555/liveMedia/RTSPServerRegister.cpp
    live555/liveMedia/ServerMediaSession.cpp
    live555/liveMedia/SimpleRTPSink.cpp
    live555/liveMedia/SimpleRTPSource.cpp
    live555/liveMedia/SIPClient.cpp
    live555/liveMedia/SRTPCryptographicContext.cpp
    live555/liveMedia/StreamParser.cpp
    live555/liveMedia/StreamParser.hh
    live555/liveMedia/StreamReplicator.cpp
    live555/liveMedia/T140TextRTPSink.cpp
    live555/liveMedia/TextRTPSink.cpp
    live555/liveMedia/TheoraVideoRTPSink.cpp
    live555/liveMedia/TheoraVideoRTPSource.cpp
    live555/liveMedia/TLSState.cpp
    live555/liveMedia/uLawAudioFilter.cpp
    live555/liveMedia/VideoRTPSink.cpp
    live555/liveMedia/VorbisAudioRTPSink.cpp
    live555/liveMedia/VorbisAudioRTPSource.cpp
    live555/liveMedia/VP8VideoRTPSink.cpp
    live555/liveMedia/VP8VideoRTPSource.cpp
    live555/liveMedia/VP9VideoRTPSink.cpp
    live555/liveMedia/VP9VideoRTPSource.cpp
    live555/liveMedia/WAVAudioFileServerMediaSubsession.cpp
    live555/liveMedia/WAVAudioFileSource.cpp
)
target_compile_definitions(
    liveMedia
    PRIVATE
        SOCKLEN_T=socklen_t
        _LARGEFILE_SOURCE=1
        _FILE_OFFSET_BITS=64
        $<$<NOT:$<TARGET_EXISTS:OpenSSL::SSL>>:NO_OPENSSL=1>
        $<$<BOOL:LIVE555_ALLOW_SERVER_PORT_REUSE>:ALLOW_SERVER_PORT_REUSE=1>
)
target_include_directories(
    liveMedia
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/live555/liveMedia/include>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/live555>
)
target_link_libraries(liveMedia PUBLIC groupsock BasicUsageEnvironment)
target_link_libraries(liveMedia PRIVATE $<TARGET_NAME_IF_EXISTS:OpenSSL::SSL>)
set_target_properties(liveMedia PROPERTIES SOVERSION ${PROJECT_VERSION})
add_library(live555::liveMedia ALIAS liveMedia)

add_executable(live555ProxyServer live555/proxyServer/live555ProxyServer.cpp)
target_link_libraries(live555ProxyServer PRIVATE liveMedia)

add_executable(live555HLSProxy live555/hlsProxy/live555HLSProxy.cpp)
target_link_libraries(live555HLSProxy PRIVATE liveMedia)

# Packaging
configure_package_config_file(
    live555Config.cmake.in live555Config.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/live555
    NO_SET_AND_CHECK_MACRO
    NO_CHECK_REQUIRED_COMPONENTS_MACRO
)
write_basic_package_version_file(
    live555ConfigVersion.cmake COMPATIBILITY ExactVersion
)

# Installation
install(TARGETS BasicUsageEnvironment groupsock liveMedia EXPORT live555Targets)
install(TARGETS live555ProxyServer live555HLSProxy)
install(
    DIRECTORY live555/UsageEnvironment/include/
              live555/BasicUsageEnvironment/include/ live555/liveMedia/include/
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/live555
)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/live555Config.cmake
              ${CMAKE_CURRENT_BINARY_DIR}/live555ConfigVersion.cmake
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/live555
)
install(
    EXPORT live555Targets
    NAMESPACE live555::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/live555
)
export(EXPORT live555Targets NAMESPACE live555::)
