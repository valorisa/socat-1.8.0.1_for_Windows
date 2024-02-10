![Screenshot_2023-11-08-23-38-22-47_40deb401b9ffe8e1df2f1cc5ba480b12](https://github.com/valorisa/socat-1.7.4.4_for_Windows/assets/13067566/c562ce4c-64e6-463b-8863-e9dd8e30d053)

socat is a relay for bidirectional data transfer between two independent data
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
  
# socat-1.8.0.0 for Windows 7, 8.1, 10 & 11 & Server

socat 1.8.0.0-x86_64 for Windows 7, 8.1, 10 & 11 & Server
[2023-30-11]

The procedure for those who want to compile from the source files.

Otherwise for the others, there is a ready-made file **'socat-1.8.0.0.rar'**.

First of all, if it is not done yet, download and install Cygwin (last version) : <https://www.cygwin.com/setup-x86_64.exe>

Install additional Cygwin packages
==================================

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

Please, don't forget to download socat source from <http://www.dest-unreach.org/socat/>

From Cygwin 3.5.0
=================

run **Cygwin** and execute the following commands :

cd / &&  cd cygdrive/c/Users/<your_username>/Desktop

wget <http://www.dest-unreach.org/socat/download/socat-1.8.0.0.tar.gz>

tar -xvzf socat-1.8.0.0.tar.gz

cd socat-1.8.0.0

./configure

make

make install

From Windows Explorer
=====================

After compilation, copy 'socat-1.8.0.0' directory to %ProgramFiles% or an other location. You have to copy the directory totally and not only 'socat.exe', otherwise it won't work.


Addendum
========
from the Mugane's comment :

May also want to add that it is best to use Powershell (as Admin) to install these packages for cygwin if using cyg-get:
```
cyg-get gcc-g++ gcc-core make gcc-fortran gcc-objc gcc-objc++ libkrb5-devel libkrb5_3 libreadline-devel libssl-devel libwrap-devel tcp_wrappers
```
If you don't use powershell, and try to install from cygwin itself (even as Administrator) you may run into gnarly cryptic missing dll errors and end up needing to remove/reinstall cygwin itself to correct the problems.

If users don't have cygwin, I recommend [chocolatey](https://chocolatey.org/install) (again from Powershell as admin):
```
choco install -y cygwin cyg-get
```
