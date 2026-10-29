add_definitions(-DTRANSLATION_DOMAIN=\"kdeconnect-plugins\")

add_subdirectory(ping)
add_subdirectory(connectivity-report)
add_subdirectory(remotecommands)
add_subdirectory(remotecontrol)
add_subdirectory(remotesystemvolume)
add_subdirectory(clipboard)
add_subdirectory(runcommand)


if(NOT WIN32)
    # https://bugs.kde.org/show_bug.cgi?id=444612
    add_subdirectory(battery)
endif()


if((WIN32 AND MSVC AND ("${CMAKE_SYSTEM_VERSION}" VERSION_GREATER_EQUAL 10.0.17763.0)) OR (NOT WIN32 AND NOT APPLE))
    add_subdirectory(mpriscontrol)
endif()
add_subdirectory(mprisremote)
if(NOT APPLE)
    add_subdirectory(lockdevice)
endif()
add_subdirectory(contacts)
add_subdirectory(telephony)
add_subdirectory(share)
add_subdirectory(remotekeyboard)
add_subdirectory(notifications)
add_subdirectory(findmyphone)
add_subdirectory(mousepad)
add_subdirectory(screensaver-inhibit)

add_subdirectory(sftp)

if(WITH_PULSEAUDIO)
    add_subdirectory(pausemusic)
    add_subdirectory(systemvolume)
endif()

# If we split notifications per plugin, in several notifyrc files, they won't
# appear in the same group in the Notifications KCM
install(FILES kdeconnect.notifyrc DESTINATION ${KDE_INSTALL_KNOTIFYRCDIR})
