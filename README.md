# Socat for Windows
Socat 1.7.4.1-x86_64 for Windows
[2021-01-10]

The procedure for those who want to compile from the source files. 

Otherwise for the others, there is a ready-made file 'socat-1.7.4.1.rar'.

First of all, download and install Cygwin : https://www.cygwin.com/setup-x86_64.exe

Install additional Cygwin packages :
– gcc-g++
– gcc-core
– cygwin32-gcc-g++
– cygwin32-gcc-core
– make

Download socat source from http://www.dest-unreach.org/socat/

wget http://www.dest-unreach.org/socat/download/socat-1.7.4.1.tar.gz

And from Cygwin 3.1.7 : 

tar -xvzf socat-1.7.4.1.tar.gz

cd socat-1.7.4.1

./configure

make

make install
