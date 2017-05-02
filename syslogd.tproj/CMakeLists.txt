project(syslogd)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -nostdinc")

include_directories(
	${CMAKE_SOURCE_DIR}/src/libinfo/gen.subproj
	${CMAKE_SOURCE_DIR}/src/libnotify
	${CMAKE_SOURCE_DIR}/src/launchd/liblaunch
	${CMAKE_SOURCE_DIR}/src/external/syslog/libsystem_asl.tproj/include
	${CMAKE_SOURCE_DIR}/src/external/libdispatch
	${CMAKE_CURRENT_SOURCE_DIR}/../aslcommon
	${CMAKE_CURRENT_BINARY_DIR}/../aslcommon
)

add_definitions(-DPRIVATE)

set(syslogd_sources
	asl_action.c
	bsd_in.c
	bsd_out.c
	daemon.c
	dbserver.c
	klog_in.c
	remote.c
	syslogd.c
	udp_in.c
	$<TARGET_OBJECTS:aslcommon>
	${CMAKE_CURRENT_BINARY_DIR}/../aslcommon/asl_ipcServer.c
)

add_darling_executable(syslogd ${syslogd_sources})

install(TARGETS syslogd DESTINATION libexec/darling/usr/sbin)
install(FILES asl.conf.osx RENAME asl.conf DESTINATION libexec/darling/etc)
install(FILES com.apple.syslogd.plist DESTINATION libexec/darling/System/Library/LaunchDaemons)
install(FILES syslog.conf DESTINATION libexec/darling/etc)
install(FILES syslogd.8 DESTINATION libexec/darling/usr/share/man/man8)
install(FILES syslog.conf.5 DESTINATION libexec/darling/usr/share/man/man5)
