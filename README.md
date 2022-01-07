# Socat for Windows
Socat 1.7.4.2-x86_64 for Windows
[2022-01-07]

The procedure for those who want to compile from the source files. 

Otherwise for the others, there is a ready-made file 'socat-1.7.4.2.rar'.

First of all, download and install Cygwin : https://www.cygwin.com/setup-x86_64.exe

Install additional Cygwin packages :

– gcc-g++

– gcc-core

– cygwin32-gcc-g++

– cygwin32-gcc-core

– make

Download socat source from http://www.dest-unreach.org/socat/

From Cygwin 3.3.3 : 

wget http://www.dest-unreach.org/socat/download/socat-1.7.4.2.tar.gz

tar -xvzf socat-1.7.4.2.tar.gz

cd socat-1.7.4.2

./configure

make

make install
