cmake_minimum_required(VERSION 3.1)

project(QBeat LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Qt5 COMPONENTS Core Quick REQUIRED)
find_package(ZLIB REQUIRED)

add_executable(${PROJECT_NAME} 
  util.cpp
  settings.cpp
  main.cpp
  actions.cpp
  json/download.cpp
  json/mod.cpp
  apis/beatmodsv1.cpp
  qml.qrc
)  

target_compile_definitions(${PROJECT_NAME} PRIVATE $<$<OR:$<CONFIG:Debug>,$<CONFIG:RelWithDebInfo>>:QT_QML_DEBUG>)
target_link_libraries(${PROJECT_NAME} PRIVATE Qt5::Core Qt5::Gui Qt5::Qml minizip)

install( TARGETS ${PROJECT_NAME}
   RUNTIME DESTINATION .
   ARCHIVE DESTINATION .
	)

# TODO: Bit nasty
install( FILES
  ${CMAKE_CURRENT_SOURCE_DIR}/../scripts/bs-linux-is-wine-valid.sh
  ${CMAKE_CURRENT_SOURCE_DIR}/../scripts/bs-linux-modfix.sh
  ${CMAKE_CURRENT_SOURCE_DIR}/../scripts/bs-linux-setup-wine.sh
   DESTINATION .
   PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE
   )
install( FILES
  ${CMAKE_CURRENT_SOURCE_DIR}/LICENSE
  DESTINATION .
  )

install( FILES
 $<TARGET_FILE:Qt5::Core>
 $<TARGET_FILE_DIR:Qt5::Core>/libQt5Core.so
 $<TARGET_FILE_DIR:Qt5::Core>/libQt5Core.so.5
 $<TARGET_FILE:Qt5::Gui>
 $<TARGET_FILE_DIR:Qt5::Core>/libQt5Gui.so
 $<TARGET_FILE_DIR:Qt5::Core>/libQt5Gui.so.5
 $<TARGET_FILE:Qt5::Network>
 $<TARGET_FILE_DIR:Qt5::Core>/libQt5Network.so
 $<TARGET_FILE_DIR:Qt5::Core>/libQt5Network.so.5
 $<TARGET_FILE:Qt5::Qml>
 $<TARGET_FILE_DIR:Qt5::Core>/libQt5Qml.so
 $<TARGET_FILE_DIR:Qt5::Core>/libQt5Qml.so.5
 DESTINATION . )

