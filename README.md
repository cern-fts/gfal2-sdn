gfal2-plugin-sdn
================
This plugin hooks to gfal2 events in order to provide feedback to an SDN
controller before a copy takes place.

How to build
------------
* Checkout the code
```bash
git clone git@github.com:cern-it-sdc-id/gfal2-sdn.git
```
* Add the Data Management Clients development repository
```bash
wget http://grid-deployment.web.cern.ch/grid-deployment/dms/lcgutil/repos/lcgutil-continuous-el6.repo -P /etc/yum.repos.d
```
* Install the dependencies
```bash
yum install cmake gfal2-devel libcurl-devel
```
* Create a directory for the build (remember to do it outside the source directory, or add it to .gitignore)
```bash
mkdir build
```

* Run cmake, and compile
```bash
cd build
cmake -DCMAKE_BUILD_TYPE=DEBUG ..
make
```

How to test
-----------
First of all, install the complete set of gfal2 plugins, and the gfal2-util package
```bash
yum install gfal2-all gfal2-util
```

To make sure everything is as expected, create your X509 proxy as usual, and run

```bash
gfal-ls gsiftp://your-favourite-endpoint/path
```

Then, you will have to create a symlink to `build/src/libgfal_plugin_sdn.so` inside `/usr/lib64/gfal2-plugins`, and to `dist/etc/gfal2.d/sdn_plugin.conf` inside `/etc/gfal2.d`

With this setup, the SDN plugins is ready to be loaded. A very simple way of telling is to run the following copy

```bash
gfal-copy -fvv gsiftp://endpoint1/path gsiftp://endpoint2/path
```

`-f ` tells gfal2 to overwrite the destination if it exists, and `-vv` tells gfal2-util to be a bit more verbose (`-v` only warnings, `-vv` warnings and messages, `-vvv` debug)

If the plugin is correctly installed and loaded, you will be able to see these lines in the output:

```
INFO     [gfal_module_load] plugin /usr/lib64/gfal2-plugins///libgfal_plugin_sdn.so loaded with success
```

Which means gfal2 saw the plugin and loaded it.

```
INFO     SDN event listener registered
```
Which means the SDN plugin injected its own event listener before starting the copy.

```
WARNING  Between endpoint1 and endpoint2 1 files with a total size of 123456 bytes
```
With is printed by the plugin _before_ the copy starts.

