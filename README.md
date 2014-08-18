vmdk
====

Recreate parts of a vmdk descriptor file from sparse files and the vmware.log

This program helps get the information needed to recreate a lost vmdk descriptor file for vmware fusion on a mac.

To build this application use the following command line:

```bash
gcc vmdk.c -o vmdk
```

To run the program against each of the vmdk files in the virtual machine directory I use the following command line:

```bash
find . -iname "*vmdk" -exec ~/vmdk "{}" \;
```

This should create output like:

```bash
richard$ find . -iname "*vmdk" -exec ~/vmdk "{}" \;
RW 4192256 SPARSE "winxp-s001.vmdk"
RW 4192256 SPARSE "winxp-s002.vmdk"
RW 4192256 SPARSE "winxp-s003.vmdk"
RW 3807264 SPARSE "winxp-s004.vmdk"
RW 4192256 SPARSE "winxp-s005.vmdk"
RW 395232 SPARSE "winxp-s006.vmdk"
RW 4192256 SPARSE "winxp-s007.vmdk"
RW 4192256 SPARSE "winxp-s008.vmdk"
RW 4192256 SPARSE "winxp-s009.vmdk"
RW 4192256 SPARSE "winxp-s010.vmdk"
RW 4192256 SPARSE "winxp-s011.vmdk"
RW 10240 SPARSE "winxp-s012.vmdk"
RW 4192256 SPARSE "winxp-s013.vmdk"
RW 4192256 SPARSE "winxp-s014.vmdk"
RW 4192256 SPARSE "winxp-s015.vmdk"
RW 4192256 SPARSE "winxp-s016.vmdk"
RW 4192256 SPARSE "winxp-s017.vmdk"
RW 4192256 SPARSE "winxp-s018.vmdk"
RW 4192256 SPARSE "winxp-s019.vmdk"
RW 4192256 SPARSE "winxp-s020.vmdk"
RW 4192256 SPARSE "winxp-s021.vmdk"
RW 4192256 SPARSE "winxp-s022.vmdk"
RW 20480 SPARSE "winxp-s023.vmdk"
# winxp.vmdk file does not have the correct magic number 69442023
```
There is one other peice of information that is required to finish off the vmdk file, that is the disk geometry for the virtual hard disk.

The disk geometry can be found in one of the vmware.log files, try the following command to find it:
```bash
richard$ grep 'Geo (' *.log 
vmware.log:2014-08-18T20:05:58.197 .... Windows XP.vmwarevm/winxp.vmdk Geo (83220/16/63) BIOS Geo (5221/255/63)
```
with this information you can create the following lines in your vmdk file:
```bash
ddb.geometry.cylinders = "83220"
ddb.geometry.heads = "16"
ddb.geometry.sectors = "63"
```
Putting it all together we end up with a vmdk file like the following:
```bash
richard$ cat winxp.vmdk 
# Disk DescriptorFile
version=1
encoding="UTF-8"
CID=07f146f3
parentCID=ffffffff
isNativeSnapshot="no"
createType="twoGbMaxExtentSparse"

# Extent description
RW 4192256 SPARSE "winxp-s001.vmdk"
RW 4192256 SPARSE "winxp-s002.vmdk"
RW 4192256 SPARSE "winxp-s003.vmdk"
RW 3807264 SPARSE "winxp-s004.vmdk"
RW 4192256 SPARSE "winxp-s005.vmdk"
RW 395232 SPARSE "winxp-s006.vmdk"
RW 4192256 SPARSE "winxp-s007.vmdk"
RW 4192256 SPARSE "winxp-s008.vmdk"
RW 4192256 SPARSE "winxp-s009.vmdk"
RW 4192256 SPARSE "winxp-s010.vmdk"
RW 4192256 SPARSE "winxp-s011.vmdk"
RW 10240 SPARSE "winxp-s012.vmdk"
RW 4192256 SPARSE "winxp-s013.vmdk"
RW 4192256 SPARSE "winxp-s014.vmdk"
RW 4192256 SPARSE "winxp-s015.vmdk"
RW 4192256 SPARSE "winxp-s016.vmdk"
RW 4192256 SPARSE "winxp-s017.vmdk"
RW 4192256 SPARSE "winxp-s018.vmdk"
RW 4192256 SPARSE "winxp-s019.vmdk"
RW 4192256 SPARSE "winxp-s020.vmdk"
RW 4192256 SPARSE "winxp-s021.vmdk"
RW 4192256 SPARSE "winxp-s022.vmdk"
RW 20480 SPARSE "winxp-s023.vmdk"

# The Disk Data Base 
#DDB

ddb.adapterType = "ide"
ddb.geometry.cylinders = "83220"
ddb.geometry.heads = "16"
ddb.geometry.sectors = "63"
ddb.longContentID = "ca794c2c629c72707e7dce1507f146f3"
ddb.toolsVersion = "9410"
ddb.uuid = "60 00 C2 91 50 a7 e8 52-8c 26 00 94 23 33 12 f3"
ddb.virtualHWVersion = "4"
```
Credits
-------
The inspiration for this, apart from a broken VM with my alarm system programming software, was the [libvmdk](https://code.google.com/p/libvmdk/) project and the [VMWare documentation](https://www.vmware.com/support/developer/vddk/vmdk_50_technote.pdf?src=vmdk)
