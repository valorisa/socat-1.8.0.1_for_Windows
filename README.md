# socat-1.7.4.4 for Windows 10 & 11
socat 1.7.4.4-x86_64 for Windows 7, 8.1, 10 & 11 & Server
[2022-04-11]

The procedure for those who want to compile from the source files. 

Otherwise for the others, there is a ready-made file **'socat-1.7.4.4.rar'**.

First of all, if it is not done yet, download and install Cygwin (last version) : https://www.cygwin.com/setup-x86_64.exe

Install additional Cygwin packages :
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

Please, don't forget to download socat source from http://www.dest-unreach.org/socat/

From Cygwin 3.4.9 : 
=================

run **Cygwin** and execute the following commands : 

cd / &&  cd cygdrive/c/Users/<your_username>/Desktop

wget http://www.dest-unreach.org/socat/download/socat-1.7.4.4.tar.gz

tar -xvzf socat-1.7.4.4.tar.gz

cd socat-1.7.4.4

./configure

make

make install

From Windows Explorer :
=====================
After compilation, copy 'socat-1.7.4.4' directory to %ProgramFiles% or an other location. You have to copy the directory totally and not only 'socat.exe', otherwise it won't work. 
