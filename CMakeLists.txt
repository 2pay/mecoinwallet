cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB cryptonote)

include_directories(${CMAKE_BINARY_DIR}
    src
    mecoin/external
    mecoin/include
    mecoin/src)

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
configure_file("mecoin/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  mecoin/external/miniupnpc/connecthostport.c
  mecoin/external/miniupnpc/igd_desc_parse.c
  mecoin/external/miniupnpc/minisoap.c
  mecoin/external/miniupnpc/miniupnpc.c
  mecoin/external/miniupnpc/miniwget.c
  mecoin/external/miniupnpc/minixml.c
  mecoin/external/miniupnpc/portlistingparse.c
  mecoin/external/miniupnpc/receivedata.c
  mecoin/external/miniupnpc/upnpcommands.c
  mecoin/external/miniupnpc/upnpreplyparse.c
  mecoin/src/Common/Base58.cpp
  mecoin/src/Common/CommandLine.cpp
  mecoin/src/Common/Util.cpp
  mecoin/src/Common/StringTools.cpp
  mecoin/src/Common/JsonValue.cpp
  mecoin/src/Common/ConsoleTools.cpp
  mecoin/src/Common/MemoryInputStream.cpp
  mecoin/src/Common/PathTools.cpp
  mecoin/src/Common/StdInputStream.cpp
  mecoin/src/Common/StdOutputStream.cpp
  mecoin/src/Common/StreamTools.cpp
  mecoin/src/Common/StringOutputStream.cpp
  mecoin/src/Common/StringView.cpp
  mecoin/src/Common/VectorOutputStream.cpp
  mecoin/src/crypto/blake256.c
  mecoin/src/crypto/chacha8.c
  mecoin/src/crypto/crypto-ops-data.c
  mecoin/src/crypto/crypto-ops.c
  mecoin/src/crypto/crypto.cpp
  mecoin/src/crypto/groestl.c
  mecoin/src/crypto/hash-extra-blake.c
  mecoin/src/crypto/hash-extra-groestl.c
  mecoin/src/crypto/hash-extra-jh.c
  mecoin/src/crypto/hash-extra-skein.c
  mecoin/src/crypto/hash.c
  mecoin/src/crypto/jh.c
  mecoin/src/crypto/keccak.c
  mecoin/src/crypto/oaes_lib.c
  mecoin/src/crypto/random.c
  mecoin/src/crypto/skein.c
  mecoin/src/crypto/slow-hash.c
  mecoin/src/crypto/slow-hash.cpp
  mecoin/src/crypto/tree-hash.c
  mecoin/src/CryptoNoteCore/BlockchainIndices.cpp
  mecoin/src/CryptoNoteCore/BlockchainMessages.cpp
  mecoin/src/CryptoNoteCore/BlockIndex.cpp
  mecoin/src/CryptoNoteCore/CoreConfig.cpp
  mecoin/src/CryptoNoteCore/CryptoNoteBasic.cpp
  mecoin/src/CryptoNoteCore/CryptoNoteTools.cpp
  mecoin/src/CryptoNoteCore/Currency.cpp
  mecoin/src/CryptoNoteCore/MinerConfig.cpp
  mecoin/src/CryptoNoteCore/Transaction.cpp
  mecoin/src/CryptoNoteCore/Account.cpp
  mecoin/src/CryptoNoteCore/Blockchain.cpp
  mecoin/src/CryptoNoteCore/Checkpoints.cpp
  mecoin/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  mecoin/src/CryptoNoteCore/Core.cpp
  mecoin/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  mecoin/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  mecoin/src/CryptoNoteCore/Difficulty.cpp
  mecoin/src/CryptoNoteCore/IBlock.cpp
  mecoin/src/CryptoNoteCore/Miner.cpp
  mecoin/src/CryptoNoteCore/TransactionExtra.cpp
  mecoin/src/CryptoNoteCore/TransactionPool.cpp
  mecoin/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  mecoin/src/CryptoNoteCore/TransactionUtils.cpp
  mecoin/src/InProcessNode/InProcessNode.cpp
  mecoin/src/InProcessNode/InProcessNodeErrors.cpp
  mecoin/src/NodeRpcProxy/NodeErrors.cpp
  mecoin/src/NodeRpcProxy/NodeRpcProxy.cpp
  mecoin/src/P2p/NetNodeConfig.cpp
  mecoin/src/Serialization/BinaryInputStreamSerializer.cpp
  mecoin/src/Serialization/BinaryOutputStreamSerializer.cpp
  mecoin/src/Serialization/JsonInputValueSerializer.cpp
  mecoin/src/Serialization/JsonOutputStreamSerializer.cpp
  mecoin/src/Serialization/KVBinaryInputStreamSerializer.cpp
  mecoin/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  mecoin/src/Serialization/SerializationOverloads.cpp
  mecoin/src/Transfers/BlockchainSynchronizer.cpp
  mecoin/src/Transfers/SynchronizationState.cpp
  mecoin/src/Transfers/TransfersConsumer.cpp
  mecoin/src/Transfers/TransfersContainer.cpp
  mecoin/src/Transfers/TransfersSubscription.cpp
  mecoin/src/Transfers/TransfersSynchronizer.cpp
  mecoin/src/Wallet/LegacyKeysImporter.cpp
  mecoin/src/Wallet/WalletAsyncContextCounter.cpp
  mecoin/src/Wallet/WalletErrors.cpp
  mecoin/src/WalletLegacy/KeysStorage.cpp
  mecoin/src/WalletLegacy/WalletLegacy.cpp
  mecoin/src/WalletLegacy/WalletHelper.cpp
  mecoin/src/WalletLegacy/WalletLegacySerializer.cpp
  mecoin/src/WalletLegacy/WalletLegacySerialization.cpp
  mecoin/src/WalletLegacy/WalletTransactionSender.cpp
  mecoin/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  mecoin/src/WalletLegacy/WalletUserTransactionsCache.cpp
  mecoin/src/System/ContextGroup.cpp
  mecoin/src/System/Event.cpp
  mecoin/src/System/EventLock.cpp
  mecoin/src/System/InterruptedException.cpp
  mecoin/src/System/Ipv4Address.cpp
  mecoin/src/System/TcpStream.cpp
  mecoin/src/HTTP/HttpRequest.cpp
  mecoin/src/HTTP/HttpParser.cpp
  mecoin/src/HTTP/HttpParserErrorCodes.cpp
  mecoin/src/HTTP/HttpResponse.cpp
  mecoin/src/Rpc/HttpClient.cpp
  mecoin/src/Rpc/JsonRpc.cpp
  mecoin/src/P2p/NetNode.cpp
  mecoin/src/P2p/LevinProtocol.cpp
  mecoin/src/P2p/NetNodeConfig.cpp
  mecoin/src/P2p/PeerListManager.cpp
  mecoin/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  mecoin/src/Logging/ILogger.cpp
  mecoin/src/Logging/LoggerGroup.cpp
  mecoin/src/Logging/CommonLogger.cpp
  mecoin/src/Logging/LoggerManager.cpp
  mecoin/src/Logging/FileLogger.cpp
  mecoin/src/Logging/StreamLogger.cpp
  mecoin/src/Logging/ConsoleLogger.cpp
  mecoin/src/Logging/LoggerMessage.cpp
  mecoin/src/Logging/LoggerRef.cpp
  mecoin/src/BlockchainExplorer/BlockchainExplorer.cpp
  mecoin/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  mecoin/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
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
  include_directories(mecoin/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} mecoin/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Mecoin GUI wallet")
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

include_directories(mecoin/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES mecoin/src/Platform/${PLATFORM_DIR}/System/*)
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
