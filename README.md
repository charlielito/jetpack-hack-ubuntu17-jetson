# Jetpack Hack Jetson

Tired of the need of Ubuntu 14.04  or 16.04 to use JetPack for the Jetson???? There is this hack to use it under other Ubuntu distros.

The workaround is easy, just download the JetPack binary and execute that with the flag _--noexec_, that is for example JetPack3.1:
```
./JetPack-L4T-3.1-linux-x64.run --noexec
```

It will then create at the current directory a folder named _ _installer_. In that folder there is a bash script named _start_up.sh_ that actually prevents us Ubuntu 17 users from using it. Just delete the last conditional where it checks the OS version _(open the file as root user or with sudo)_. The last part of the file should be:

```
os_version=`lsb_release -r | awk '{ printf \$2 }'`
if [ "$os_version" == "14.04" ] || [ "$os_version" == "16.04" ] ; then
    mv JetPack_Uninstaller ../
    ./Launcher "$@"
else
    rm -rf ../_installer
    xmessage -fn '-*-*-*-*-*--*-150-*-*-p-*-iso8859-1' -center "Error: JetPack must be run on Ubuntu 14.04 or 16.04 platform. Detected Ubuntu $os_version platform."
    exit 1
fi
```

Delete the whole part from **_if_** to **_fi_**. Then execute that file and finally to launch JetPack just run _Launcher_ executable (another file in the _ _installer_ folder)!!!

```
./start_up.sh
./Launcher
```

**PD: If you got the error _"Error while loading shared libraries: libpng12.so.0: cannot open shared object file: No such file or directory"_** just install it using the following:

```
sudo wget -q -O /tmp/libpng12.deb http://mirrors.kernel.org/ubuntu/pool/main/libp/libpng/libpng12-0_1.2.54-1ubuntu1_amd64.deb \
  && sudo dpkg -i /tmp/libpng12.deb \
  && sudo rm /tmp/libpng12.deb
```
