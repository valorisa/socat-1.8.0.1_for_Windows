
####################### V 1.8.0.1 :

Corrections:
	When no IP version was preferred by environment, option -4/-6, or
	address option pf, Socat version 1.8.0.0 address TCP-LISTEN did not
	accept TCP4 connections under BSD family operating systems, but only
	TCP6. To regain previous behaviour, preferring IP version 4 is now the
	default. This also fixes some other issues with bind and range options.
	Thanks to Mike Andrews for reporting this issue.
	Tests: LISTEN_4 LISTEN_6  V1800_*_RANGE  V1800_*_BIND

	Added Socat option -0 to allow version 1.8.0.0 behaviour (no preferred
	IP version).

	UDP-SENDTO, UDPLITE-SENDTO, and IP-SENDTO addresses now select an IPv4
	address in case the server name resolves to both IPv4 and IPv6
	addresses.
	Tests: V1800_*_SENDTO_RESOLV_6_4

	Guard applyopts_termios_value() with WITH_TERMIOS.
	Thanks to Kush Upadhyay from Amazon Bottlerocket team for providing the
	patch.

	In some situations xioclose() was called nested what could cause hanging
	of OpenSSL in pthread_rwlock_wrlock()

	socat 1.8.0.0 with addresses of type RECVFROM and option fork, where
	the second address failed to connect/open in the child process, entered
	a fork loop that was only stopped by FD exhaustion caused by FD leak.
	Test: RECVFROM_FORK_LOOP

	socat 1.8.0.0 had an FD leak with addresses of type RECVFROM with fork.
	Test: RECVFROM_FORK_LEAK

	With version 1.8.0.0, options ipv6-join-group and ipv6-join-source-group
	did not work.
	Thanks to Linus Luessing for reporting this bug.

	IP-SENDTO and option pf (protocol-family) with protocol name (vs.numeric
	argument) failed with message:
	E retropts_int(): trailing garbage in numerical arg of option "protocol-family"
	Test: IP_SENDTO_PF

	Fixed a possible buffer overrun with long log lines. In fact it does
	not write beyond end of buffer but lets pass excessive data to the
	write() function.
	Thanks to Heinrich Schuchardt from Canonical for reporting and sending
	a patch.

	Reworked domain name resolution, centralized IPv4/IPv6 sorting.

	Print warning about not checking CRLs in OpenSSL only in the first
	child process.

Features:
	Total inactivity timeout option -T 0 now means 0.0 seconds; up to
	version 1.8.0.0 it meant no total inactivity timeout.

	Changed socat-chain.sh, socat-mux.sh, and socat-broker.sh to work with
	older Socat versions.

	socat-mux.sh and socat-broker.sh, when run as root, now internally use
	low (512..1023) UDP ports to increase security.

	Added option ai-all (sets AI_ALL flag of getaddrinfo() resolver)

	Socks5 now also allows syntax without socks port, and supports option
	socksport.

Porting:
	Changes for building and testing on NetBSD

	New Linux distributions dislike egrep, fgrep

	When NETDB_INTERNAL is not available it should be set to -1.
	Thanks to Baruch Siach for sending a patch.

	On OpenSolaris/Illumos, isastream() is declared only in stropts.h, not
	in sys/stropts.h
	Thanks to Andy Fiddaman for sending a patch.

	On latest Illumos, compilation failed due to new unexpected SO_PROTOCOL
	implementation.
	Thanks to Andy Fiddaman for sending a patch.

Building:
	Makefile.in: procan.o build requires srcdir prefix for explicit source
	file.
	Thanks to Hongxu Jia and Andrew Schoolman for providing patches.

	Makefile.in: the CC define for procan.o build failed when CC had more
	than one word.
	Thanks to Hongxu Jia for providing an inital patch.

Testing:
	Added the optional DEVTESTS feature for developer tests with controlled
	name resolution to both IPv4 and IPV6 addresses: configure Socat with
	--enable-devtests, this provides internal resolution of domain
	dest-unreach.net with host names: localhost-4, localhost-6,
	localhost-4-6, and localhost-6-4

	test.sh: lots of corrections and improvements

	test.sh: many hardcoded sleep values were replaced by much shorter
	values tuned to performance of the platform.

	test.sh -D for output of platform/system specific defines (variables)

	test.sh: fixed ss determination; more DEFS
	
Documentation:
	Fixed a lot of typos.
	Thanks to Solomon Victorino for sending the patch.

####################### V 1.8.0.0:

Security:
	Socats OpenSSL addresses do not (and never did) check certificate
	revocation lists (CRLs). Socat now prints a warning about this.

Features:
	Added the --experimental option that enables use of features that might
	change in the future.

	Now warning messages are printed by default. If you want to see only
	errors and fatals as in previous versions, use option -d0;
	option -d4 is equivalent to -dddd and to -d -d -d -d
	The number of warnings has been reduced, e.g.removing a non existing
	file does in most cases no longer log a warning.

	Added address type internal SOCKETPAIR. This is similar to the unnamed
	PIPE address (only for internal echoing) but it provides datagram mode
	(the default) and thus keeps packet boundaries.
	Tests: SOCKETPAIR_STREAM SOCKETPAIR_DATAGRAM SOCKETPAIR_SEQPACKET
	SOCKETPAIR_BOUNDARIES

	New option -S <mask> controls catching and logging of signals that are
	not internally used by Socat.
	Tests: SIGTERM_NOLOG SIG31_LOG

	Added option ipv6-join-source-group.
	Thanks to Martin Buck and David Schweizer for sending patches.

	Added option http-version to PROXY-CONNECT address to support servers
	that are not able to handle HTTP version 1.0
	Test: PROXY_HTTPVERSION
	Feature inspired by Robin Palotai.

	New options openssl-maxfraglen and openssl-maxsendfrag for
	functions/macros SSL_CTX_set_tlsext_max_fragment_length() and
	SSL_CTX_set_max_send_fragment().
	Thanks to James Tavares for his contribution.

	Added Info log of resulting OpenSSL max fragment length.

	Implemented options rcvtimeo and sndtimeo, the first of which may be
	useful to prevent endlessly hanging DTLS connection etablishment.
	Test: RCVTIMEO_DTLS
	Feature proposed by Vladimir Nikishkin.

	The file names with -r and -R now may contain environment variable
	references.
	Test: VARS_IN_SNIFFPATH

	Socat option --statistics logs final byte and packet counter values
	before exit. Signal USR1 logs actual values.
	Tests: OPTION_STATISTICS SIGUSR1_STATISTICS

	Added option sitout-eio to specify a timerange in which EIO on the pty
	of a sub process is tolerated.
	Red Hat issue 1853102 related.
	Thanks to Jonathan Casiot for sending an initial patch.

	Socat now installs as socat1 and is referenced by symbolic link socat,
	same with man page (socat1.1 by socat.1)

	New option children-shutup[=1|2...] decreases severity of log
	messages in LISTEN and CONNECT type sub processes.
	Test: CHILDREN_SHUTUP

	New option retrieve-vlan for supporting VLANs in INTERFACE addresses:
	Linux normally keeps VLAN tags in outgoing raw packets, but appears to
	strip them from incoming packets and makes them available in
	PACKET_AUXDATA ancillary messages only.
	Up do version 1.7.4.5 Socat did not handle this situation, so the VLAN
	tags where effectively stripped off incoming packets.
	With this option Socat restores the VLAN tag.
	Feature inspired by Zhao Dong.

	Socket option SO_REUSEADDR is now automatically applied to TCP LISTEN
	addresses. reuseaddr= restores the old behaviour.
	Tests: TCP4_REUSEADDR OPENSSL_6_REUSEADDR REUSEADDR_NULL

	TCP based client addresses now try all results of name resolution until
	a connection attempt succeeded.
	Tests: TRY_ADDRS_4 TRY_ADDRS_4_6
	Feature recommended by Anand Buddhdev.

	configure option --enable-default-ipv allows to specify at build time if
	IPv4, IPv6, or none of these is the preferred default; this is related
	to environment variables SOCAT_PREFERRED_RESOLVE_IP and
	SOCAT_DEFAULT_LISTEN_IP, and to Socat option -4, -6.
	Furthermore, mechanism of IPv4 vs.IPv6 selection has been reworked.
	When no IP version is preferred by these mechanism, passive Socat
	addresses (LISTEN, RECV, RECVFROM) default to IPv6 because it might
	support both versions (but checkout option ipv6-v6only).
	For client addresses, when one of these mechanisms applies and name
	resolution gives addresses of both IP versions, the addresses of the
	preferred versions are tried first.

	New option ai-addrconfig sets or unsets the AI_ADDRCONFIG flag of the
	resolver to prevent name resolution to address families that are not
	available in the network configuration. Default value is 1 in case the
	resolver does not get an address family hint.

	Flag AI_PASSIVE is now automatically applied for LISTEN, RECV, and
	RECVFROM type addresses, and with bind option. In addition to its
	application to the getaddrinfo() function, when this flag is set while
	no IP version is preferred by build, environment, option, or address
	type, Socat chooses IPv6 because this might activate both versions (but
	check option ipv6-v6only).
	Added option ai-passive to control this flag explicitely.

	New option ai-v4mapped (v4mapped) sets or unsets the AI_V4MAPPED flag
	of the resolver. For Socat addresses requiring IPv6 addresses, this
	resolves IPv4 addresses to the approriate IPv6 address [::ffff:*:*].

	DNS resolver Options (res-*) are now set for the complete open phase of
	the address, not per getaddrinfo() invocation.

	Added the netns option that tries to open an address in the given
	network namespace.
	Tests: NETNS NETNS_EXEC

	New address ACCEPT-FD (ACCEPT) expects a listening file descriptor
	passed from parent, and accepts one or more connections for data
	transfer. This can be used with "inetd mode" of systemd.
	Test: ACCEPT_FD

	Added experimental socks5 TCP client support (connect,bind); syntax:
	SOCKS5-CONNECT:<socks-server>:<socks-port>:<target-host>:<target-port>
	SOCKS5-LISTEN:<socks-server>:<socks-port>:<listen-host>:<listen-port>
	Thanks to Charlie Svensson and others for contributions.

	New address types POSIXMQ-RECEIVE, POSIXMQ-READ, POSIXMQ-SEND, and
	POSIXMQ-BIDIRECTIONAL (Linux only, experimental), and option
	posixmq-priority
	Tests: LINUX_POSIXMQ_READ_PRIO LINUX_POSIXMQ_RECV_FORK
	LINUX_POSIXMQ_RECV_MAXCHILDREN LINUX_POSIXMQ_SEND_MAXCHILDREN

	New address SHELL invokes a shell but without the overhead of SYSTEM

	Added options res-retrans and res-retry that make use of undocumented
	resolver variables to set the retransmission time interval resp.the
	number of times to retransmit.
	Disable them and the old res-* opts with: ./configure --disable-resolve

	Added option res-nsaddr that overrides /etc/resolv.conf nameserver
	address based on an undocumented resolver feature.

	New option chdir changes the working directory of the address to the
	given path, only during the open stage.
	Tests: CHDIR_ON_CREATE CHDIR_ON_SYSTEM

	Option umask now applies only during opening of its very address, not
	for the lifetime of the process; the original umask is restored
	afterwards.
	Tests: UMASK_ON_CREATE UMASK_ON_SYSTEM

	Added option unix-bind-tempname (bind-tempname) to allow UNIX (and
	ABSTRACT) client addresses to bind to unique addresses even when
	invoked	in forked off sub processes.
	Tests: UNIX_LISTEN_CONNECT_BIND_TEMPNAME UNIX_LISTEN_CLIENT_BIND_TEMPNAME
	UNIX_RECVFROM_CLIENT_BIND_TEMPNAME UNIX_RECVFROM_SENDTO_BIND_TEMPNAME
	ABSTRACT_LISTEN_CONNECT_BIND_TEMPNAME ABSTRACT_LISTEN_CLIENT_BIND_TEMPNAME
	ABSTRACT_RECVFROM_CLIENT_BIND_TEMPNAME ABSTRACT_RECVFROM_SENDTO_BIND_TEMPNAME
	Thanks to Kai Lüke for sending an initial patch.

	New option f-setpipe-sz (pipesz) sets the pipe size on systems that
	provide ioctl F_SETIPE_SZ.
	Filan prints the current value.
	Tests: STDIN_F_SETPIPE_SZ EXEC_F_SETPIPE_SZ

	Bidirectional PIPE addresses may block on writing a data chunk larger
	than pipe buffer. Socat now tries to detect if transfer block size is
	large enough and issues a warning.

	Added direct support of DCCP protocol, new addresses:
	DCCP-CONNECT (DCCP)
	DCCP-LISTEN (DCCP-L)
	DCCP4-CONNECT (DCCP4)
	DCCP4-LISTEN (DCCP4-L)
	DCCP6-CONNECT (DCCP6)
	DCCP6-LISTEN (DCCP6-L)
	New option: dccp-set-ccid (ccid)

	Support for UDP-Lite protocol, new addresses:
	UDPLITE-CONNECT
	UDPLITE-LISTEN
	UDPLITE-DATAGRAM
	UDPLITE-RECV
	UDPLITE-RECVFROM
	UDPLITE-SENDTO
	All these are also available in UDPLITE4-* and UDPLITE6-* form;
	options udplite-recv-cscov and udplite-send-cscov.

	Procan now prints info about CC and __STDC_VERSION__, about FD_SETSIZE,
	value of SO_PROTOCOL/SO_PROTOTYPE and some other defines, definitions
	of many C types, and the actual umask.

	Procan tries to find the name of the controlling terminal, on Linux it
	reads info from /proc/self/stat and searches for a device with matching
	major and minor numbers.

	Added socat-chain.sh that makes it possible to stack protocols, e.g. to
	drive socks through TLS, or to use TLS over a serial line.
	Tests: SOCAT_CHAIN_SOCKS4 SOCAT_CHAIN_SSL_PTY

	Added script socat-mux.sh that performs n-to-1 / 1-to-n communications
	using two Socat instances with multicasting.
	Tests: SOCAT_MUX

Corrections:
	When a sub process (EXEC, SYSTEM) terminated with exit code other than
	0, its last sent data might have been lost depending on timing of read/
	write and SIGCHLD in Socat.
	Now the SIGCHLD handler does not simply terminate Socat in this case,
	but remembers the failure and allows further processing.
	Thanks to Luke Jones for reporting this issue.

	Now catching the case of empty SNI host to prevent OpenSSL error.
	This is related to Red Hat issue 2081414.

	Better formatted help output; address keywords in help output are now
	printed in uppercase.

	In previous Socat versions errors EPIPE and ECONNRESET on read() were
	handled at warning level, thus not automatically leading to termination
	with exit code 1. Beginning with this release these conditions are
	handled as errors with termination and exit code 1 to not pretend
	success on possible data loss.
	Problem reported by Scott Burkett.

	In previous Socat versions errors on shutdown() were ignored (info
	level).
	Now Socat handles EPIPE and ECONNRESET as errors to indicate possible
	failure of data transfer.

	INTERFACE addresses did not accept options of INTERFACE group (for
	historical reasons they were only available with TUN addresses).

	Opening addresses did not check if they support all directions expected
	by Socat. Now an error is printed when, e.g,, a read-only type address
	is opened for writing.

	A lot of minor corrections, e.g., catch readline() errors in filan,
	detect byte order in procan
	Test: EXEC_SIGINT

	OpenSSL cipherlist option did not override global openssl.cnf settings.
	Now SSL_CTX_set_cipher_list() is called before
	SSL_CTX_use_certificate_chain_file().
	Thanks to Hiroshi Sakurai for reporting the problem and suggesting this
	solution.

	Fixed option sourceport with UDP6-DATAGRAM.

	Some client addresses (e.g. TCP-CONNECT) take the fork option for
	automatically spawning new connections, however the max-children option
	was not applied.

	Fixed the end-close option, it just did not work.

	In configure.ac was a direct call to gcc instead of $CC which broke
	cross compiling.
	Thanks to Fergus Dall for sending a patch.

Coding:
	Introduced groups_t instead of uint32_t, for more flexibility.

	Rearranged option group bits to only require 32 bits on older systems.

	Make gcc happy, replace strncat with "manual" copying

	On addresses like UDP-RECVFROM with fork option every packet causes a
	new child process which then reads the packet. The parent process must
	wait until the packet has been read before checking again. The former
	synchronization mechanism using SIGUSR1 is now replaced by a
	socketpair. SIGUSR1 is no longer used for internal synchronization.
	Tests: UDP4_FORK UDP6_FORK UNIX_FORK

	Renamed xioopts_t to xioparms_t to avoid confusion with xioopts module.

	Moved multicast related code from xioopts.c to xio-ip.c and xio-ip6.c

	Pointers of type struct single are now always called sfd.

Porting:
	Removed Config/ because its contents have not been maintained for many
	years.

	Try to not receive outgoing packets on raw (PF_PACKET) sockets - use
	PACKET_IGNORE_OUTGOING socket options when available.
	Test: INTERFACE_IGNOREOUTGOING

	Renewed port to OpenBSD:
	Guard OPENSSL_INIT_SETTINGS; and minor changes.

	Thanks to Paul Hunt for sending a fix of the configure
	--enable-openssl-base processing.

	Enable direct largefile support on "smaller" systems per
	_FILE_OFFSET_BITS and _LARGE_FILES.
	Thanks to Fergus Dall for sending a patch.

	Some corrections for better 32bit systems support.

Testing:
	Removed obselete parts from test.sh

	test.sh: Introduced function checkcond

	Renamed test.sh option -foreign to -internet

Documentation:
	Removed obselete file doc/xio.help

	Added doc for option ipv6-join-group (ipv6-add-membership)
	Thanks to Martin Buck for sending the patch.

	Renamed xiogetpacketsrc() to xiogetancillary()

	On bad parameter number now print syntax.

####################### V 1.7.4.5 (not released):

Corrections:
	On connect() failure and in some other situations Socat tries to get
	detailled information about the error with recvmsg(). Error return of
	this function is now logged as Info instead of Warn.

	Tests of the correction of the "IP_ADD_SOURCE_MEMBERSHIP but not struct
	ip_mreq_source" issue left an #undef in xiosysincludes.h that disabled
	the ip-add-source-membership option.
	Thanks to Benjamin Poirier for sending a patch.

	Fixed a bug in dalan module that caused SIGSEGV in, e.g.,
	SOCKET-LISTEN:1:1:'"/tmp/sock"'
	Test: DALAN_NO_SIGSEGV

	The retry option with some address types (TCP) did not close() the
	sockets after failed attempts, resulting in an FD leak.

	Filan: Corrected some syntax error messages

	Filan: Fixed a bug introduced in 1.7.4.4 that broke displaying
	TCP/UDP on options -s, -S
	Test: FILAN_SHORT_TCP

	Filan: If IP protocol type cannot be retrieved, display at least the
	socket type

	Filan: Fixed diag_set() call in filan_main.c, bug popped up with C23.
	Thanks to Cristian Rodríguez from openSUSE for reporting this issue.

	Querying the vsock Context Identifier (CID) requires an FD from opening
	/dev/vsock.
	Thanks to Volker Simonis for sending a patch.

	Fixed an internal FD leak in the EXEC,SYSTEM addresses.

	The FDs of the socketpair that queues messages from signal handlers
	lacked FD_CLOEXEC and thus leaked into EXEC and SYSTEM child processes.

	Option stderr on addresses EXEC and SYSTEM uses a temporary FD. It
	lacked the FD_CLOEXEC setting and thus leakt into child processes.

	Restoring of STDIO tty settings failed on Solaris type operating
	systems.
	Thanks to Gordon W.Ross for reporting and fixing this issue.
	Test: RESTORE_TTY

	The OpenSSL client SNI parameter, when not explicitely specified, is
	derived from option commonname or rom target server name. This is not
	useful with IP addresses, which Socat now checks and avoids.

	Socat options -L and -W create lock files using mkstemp(), so they had
	permissions 600. There does not seem to be a good reason for this
	restrictive mode. Furthermore Silla Rizzoli experienced that Minicom
	ignores lock files with mode 600, so it is set to 644 now.

	Procan tries to find out VSOCK CID only when running as root

	The mechanism for deferring logs from signal handlers had an issue that
	caused lots of unwanted recvfrom() calls.

	Do not try to remove abstract UNIX socket entries after use.

Features:
	VSOCK, VSOCK-L support options pf, socktype, prototype (currently
	useless)

Coding:
	New Environment variable SOCAT_TRANSFER_WAIT that Socat sleep before
	starting the data transfer loop. Useful, e.g., to accumulate multiple
	packets in a receiving datagram socket before starting to process them.

	"//" comments were used for disabling experimental code. These lines
	have now been removed or disabled in other ways to make Socat compile
	with C89/C90 standard again.

	fcntl() trace prints flags now in hexadecimal.

	Stream dump options -r and -R now open their pathes with CLOEXEC to
	prevent leaking into sub processes.
	Test: EXEC_SNIFF

	Stream dump write now warn on write errors and partial writes (but
	still do not recover).

	Removed trailing white space from *.h and *.c files.

Porting:
