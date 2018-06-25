cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB cryptonote)

include_directories(${CMAKE_BINARY_DIR}
    src
    danicoin/external
    danicoin/include
    danicoin/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "")
configure_file("danicoin/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  danicoin/external/miniupnpc/connecthostport.c
  danicoin/external/miniupnpc/igd_desc_parse.c
  danicoin/external/miniupnpc/minisoap.c
  danicoin/external/miniupnpc/miniupnpc.c
  danicoin/external/miniupnpc/miniwget.c
  danicoin/external/miniupnpc/minixml.c
  danicoin/external/miniupnpc/portlistingparse.c
  danicoin/external/miniupnpc/receivedata.c
  danicoin/external/miniupnpc/upnpcommands.c
  danicoin/external/miniupnpc/upnpreplyparse.c
  danicoin/src/Common/Base58.cpp
  danicoin/src/Common/CommandLine.cpp
  danicoin/src/Common/Util.cpp
  danicoin/src/Common/StringTools.cpp
  danicoin/src/Common/JsonValue.cpp
  danicoin/src/Common/ConsoleTools.cpp
  danicoin/src/Common/MemoryInputStream.cpp
  danicoin/src/Common/PathTools.cpp
  danicoin/src/Common/StdInputStream.cpp
  danicoin/src/Common/StdOutputStream.cpp
  danicoin/src/Common/StreamTools.cpp
  danicoin/src/Common/StringOutputStream.cpp
  danicoin/src/Common/StringView.cpp
  danicoin/src/Common/VectorOutputStream.cpp
  danicoin/src/crypto/blake256.c
  danicoin/src/crypto/chacha8.c
  danicoin/src/crypto/crypto-ops-data.c
  danicoin/src/crypto/crypto-ops.c
  danicoin/src/crypto/crypto.cpp
  danicoin/src/crypto/groestl.c
  danicoin/src/crypto/hash-extra-blake.c
  danicoin/src/crypto/hash-extra-groestl.c
  danicoin/src/crypto/hash-extra-jh.c
  danicoin/src/crypto/hash-extra-skein.c
  danicoin/src/crypto/hash.c
  danicoin/src/crypto/jh.c
  danicoin/src/crypto/keccak.c
  danicoin/src/crypto/oaes_lib.c
  danicoin/src/crypto/random.c
  danicoin/src/crypto/skein.c
  danicoin/src/crypto/slow-hash.c
  danicoin/src/crypto/slow-hash.cpp
  danicoin/src/crypto/tree-hash.c
  danicoin/src/CryptoNoteCore/BlockchainIndices.cpp
  danicoin/src/CryptoNoteCore/BlockchainMessages.cpp
  danicoin/src/CryptoNoteCore/BlockIndex.cpp
  danicoin/src/CryptoNoteCore/CoreConfig.cpp
  danicoin/src/CryptoNoteCore/CryptoNoteBasic.cpp
  danicoin/src/CryptoNoteCore/CryptoNoteTools.cpp
  danicoin/src/CryptoNoteCore/Currency.cpp
  danicoin/src/CryptoNoteCore/MinerConfig.cpp
  danicoin/src/CryptoNoteCore/Transaction.cpp
  danicoin/src/CryptoNoteCore/Account.cpp
  danicoin/src/CryptoNoteCore/Blockchain.cpp
  danicoin/src/CryptoNoteCore/Checkpoints.cpp
  danicoin/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  danicoin/src/CryptoNoteCore/Core.cpp
  danicoin/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  danicoin/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  danicoin/src/CryptoNoteCore/Difficulty.cpp
  danicoin/src/CryptoNoteCore/IBlock.cpp
  danicoin/src/CryptoNoteCore/Miner.cpp
  danicoin/src/CryptoNoteCore/TransactionExtra.cpp
  danicoin/src/CryptoNoteCore/TransactionPool.cpp
  danicoin/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  danicoin/src/CryptoNoteCore/TransactionUtils.cpp
  danicoin/src/InProcessNode/InProcessNode.cpp
  danicoin/src/InProcessNode/InProcessNodeErrors.cpp
  danicoin/src/NodeRpcProxy/NodeErrors.cpp
  danicoin/src/NodeRpcProxy/NodeRpcProxy.cpp
  danicoin/src/P2p/NetNodeConfig.cpp
  danicoin/src/Serialization/BinaryInputStreamSerializer.cpp
  danicoin/src/Serialization/BinaryOutputStreamSerializer.cpp
  danicoin/src/Serialization/JsonInputValueSerializer.cpp
  danicoin/src/Serialization/JsonOutputStreamSerializer.cpp
  danicoin/src/Serialization/KVBinaryInputStreamSerializer.cpp
  danicoin/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  danicoin/src/Serialization/SerializationOverloads.cpp
  danicoin/src/Transfers/BlockchainSynchronizer.cpp
  danicoin/src/Transfers/SynchronizationState.cpp
  danicoin/src/Transfers/TransfersConsumer.cpp
  danicoin/src/Transfers/TransfersContainer.cpp
  danicoin/src/Transfers/TransfersSubscription.cpp
  danicoin/src/Transfers/TransfersSynchronizer.cpp
  danicoin/src/Wallet/LegacyKeysImporter.cpp
  danicoin/src/Wallet/WalletAsyncContextCounter.cpp
  danicoin/src/Wallet/WalletErrors.cpp
  danicoin/src/WalletLegacy/KeysStorage.cpp
  danicoin/src/WalletLegacy/WalletLegacy.cpp
  danicoin/src/WalletLegacy/WalletHelper.cpp
  danicoin/src/WalletLegacy/WalletLegacySerializer.cpp
  danicoin/src/WalletLegacy/WalletLegacySerialization.cpp
  danicoin/src/WalletLegacy/WalletTransactionSender.cpp
  danicoin/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  danicoin/src/WalletLegacy/WalletUserTransactionsCache.cpp
  danicoin/src/System/ContextGroup.cpp
  danicoin/src/System/Event.cpp
  danicoin/src/System/EventLock.cpp
  danicoin/src/System/InterruptedException.cpp
  danicoin/src/System/Ipv4Address.cpp
  danicoin/src/System/TcpStream.cpp
  danicoin/src/HTTP/HttpRequest.cpp
  danicoin/src/HTTP/HttpParser.cpp
  danicoin/src/HTTP/HttpParserErrorCodes.cpp
  danicoin/src/HTTP/HttpResponse.cpp
  danicoin/src/Rpc/HttpClient.cpp
  danicoin/src/Rpc/JsonRpc.cpp
  danicoin/src/P2p/NetNode.cpp
  danicoin/src/P2p/LevinProtocol.cpp
  danicoin/src/P2p/NetNodeConfig.cpp
  danicoin/src/P2p/PeerListManager.cpp
  danicoin/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  danicoin/src/Logging/ILogger.cpp
  danicoin/src/Logging/LoggerGroup.cpp
  danicoin/src/Logging/CommonLogger.cpp
  danicoin/src/Logging/LoggerManager.cpp
  danicoin/src/Logging/FileLogger.cpp
  danicoin/src/Logging/StreamLogger.cpp
  danicoin/src/Logging/ConsoleLogger.cpp
  danicoin/src/Logging/LoggerMessage.cpp
  danicoin/src/Logging/LoggerRef.cpp
  danicoin/src/BlockchainExplorer/BlockchainExplorer.cpp
  danicoin/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  danicoin/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(danicoin/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} danicoin/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Danicoin GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})
	
    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(danicoin/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES danicoin/src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
