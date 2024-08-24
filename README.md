# Socat for Windows

![Screenshot_2023-11-08-23-38-22-47_40deb401b9ffe8e1df2f1cc5ba480b12](https://github.com/valorisa/socat-1.7.4.4_for_Windows/assets/13067566/c562ce4c-64e6-463b-8863-e9dd8e30d053)

socat (SOcket CAT: netcat on steroids) is a relay for bidirectional data transfer between two independent data
channels. Each of these data channels may be a file, pipe, device (serial line
etc. or a pseudo terminal), a socket (UNIX, IP4, IP6 - raw, UDP, TCP), an
SSL socket, proxy CONNECT connection, a file descriptor (stdin etc.), the GNU
line editor (readline), a program, or a combination of two of these.
These modes include generation of "listening" sockets, named pipes, and pseudo
terminals.

Some of the examples of using socat are :

- TCP relay (one-shot or daemon)
- External socksifier
- Shell interface to Unix sockets
- IPv6 relay
- Netcat and rinetd replacement
- Redirecting TCP-oriented programs to a serial line
- Establishing a relatively secure environment (su and chroot) for running client or server shell scripts inside network connections.

 <http://www.dest-unreach.org/socat/doc/socat.html#EXAMPLES>
  
## socat-1.8.0.1 for Windows 7, 8.1, 10 & 11 & Server

socat 1.8.0.1-x86_64 for Windows 7, 8.1, 10 & 11 & Server
[2024-08-24]

The procedure for those who want to compile from the source files.

Otherwise for the others, there is one ready-made file **'socat-1.8.0.1.7z'**.
You can download it by going to : [socat-1.8.0.1.7z](https://github.com/valorisa/socat-1.8.0.1_for_Windows/blob/main/socat-1.8.0.1.7z) and proceeding by keyboard shortcut (Ctrl + Shift + s).

First of all, if it is not done yet, download and install Cygwin (last version) : <https://www.cygwin.com/setup-x86_64.exe>

## 1. **Install additional Cygwin packages**

– gcc-g++

– gcc-core

– cygwin32-gcc-g++

– cygwin32-gcc-core

– make

– gcc-fortran

– gcc-objc

– gcc-objc++

– libkrb5-devel

– libkrb5_3

– libreadline-devel

– libssl-devel

– libwrap-devel

– tcp_wrappers

To do this, let us try to answer the following question : How to do install packages on Cygwin ?
Download the Cygwin installer and run setup.exe. Click Next through the defaults and select mirror for downloading packages. Search for each package, open the appropriate category (by example Net or PHP or other), and click Skip next to each package to select it for installation.

## 2. **From Cygwin 3.5.3**

Please, don't forget to download socat source from <http://www.dest-unreach.org/socat/>

Run **Cygwin** via (Windows + R, 'mintty') and execute the following commands :

```bash
cd / && cd cygdrive/c/Users/<your_username>/Desktop [or cd / && cd %USERPROFILE%/Desktop if you use (Windows + R, 'cmd')]

wget http://www.dest-unreach.org/socat/download/socat-1.8.0.1.tar.gz

tar -xvzf socat-1.8.0.1.tar.gz

cd socat-1.8.0.1

./configure

make

make install
```

## 3. **From Windows Explorer**

After compilation, copy _'socat-1.8.0.1'_ directory to %ProgramFiles% or an other location. You have to copy the directory totally and not only 'socat.exe', otherwise it won't work. Caution : Add the socat's path from environment variables, with (Windows + R, 'sysdm.cpl', advanced system settings). Close 'mintty' and reopen it.

Note (from 'mintty' [cygwin] to verify the version number) : 
```bash
$ socat -V
socat by Gerhard Rieger and contributors - see www.dest-unreach.org
socat version 1.8.0.1 on Aug 24 2024 21:18:20
   running on CYGWIN_NT-10.0-26100 version 2024-04-03 17:25 UTC, release 3.5.3-1.x86_64, machine x86_64
features:
  #define WITH_HELP 1
  #define WITH_STATS 1
  #define WITH_STDIO 1
  #define WITH_FDNUM 1
  #define WITH_FILE 1
  #define WITH_CREAT 1
  #define WITH_GOPEN 1
  #define WITH_TERMIOS 1
  #define WITH_PIPE 1
  #define WITH_SOCKETPAIR 1
  #define WITH_UNIX 1
  #undef WITH_ABSTRACT_UNIXSOCKET
  #define WITH_IP4 1
  #define WITH_IP6 1
  #define WITH_RAWIP 1
  #define WITH_GENERICSOCKET 1
  #undef WITH_INTERFACE
  #define WITH_TCP 1
  #define WITH_UDP 1
  #undef WITH_SCTP
  #undef WITH_DCCP
  #undef WITH_UDPLITE
  #define WITH_LISTEN 1
  #undef WITH_POSIXMQ
  #define WITH_SOCKS4 1
  #define WITH_SOCKS4A 1
  #define WITH_SOCKS5 1
  #undef WITH_VSOCK
  #undef WITH_NAMESPACES
  #define WITH_PROXY 1
  #define WITH_SYSTEM 1
  #define WITH_SHELL 1
  #define WITH_EXEC 1
  #define WITH_READLINE 1
  #undef WITH_TUN
  #define WITH_PTY 1
  #define WITH_OPENSSL 1
  #undef WITH_FIPS
  #define WITH_LIBWRAP 1
  #define WITH_SYCLS 1
  #define WITH_FILAN 1
  #define WITH_RETRY 1
  #undef WITH_DEVTESTS
  #define WITH_MSGLEVEL 0 /*debug*/
  #define WITH_DEFAULT_IPV 4
```
## 4. **Addendum**

From the Mugane's comment :

May also want to add that it is best to use Powershell (as Admin) to install these packages for cygwin if using cyg-get :

```bash
cyg-get gcc-g++ gcc-core make gcc-fortran gcc-objc gcc-objc++ libkrb5-devel libkrb5_3 libreadline-devel libssl-devel libwrap-devel tcp_wrappers
```

If you don't use powershell, and try to install from cygwin itself (even as Administrator) you may run into gnarly cryptic missing dll errors and end up needing to remove/reinstall cygwin itself to correct the problems.

If users don't have cygwin, I recommend [chocolatey](https://chocolatey.org/install) (again from Powershell as admin) :

```bash
choco install -y cygwin cyg-get
```
