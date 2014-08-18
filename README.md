vmdk
====

recreate a vmdk descriptor file from sparse files and the vmware.log

This program helps get the information needed to recreate a lost vmdk descriptor file for vmware fusion on a mac.

To build this application use the following command line:

```bash
gcc vmdk.c -o vmdk
```

To run the program against each of the vmdk files in the virtual machine directory I use the following command line:

```bash
for A in `ls *.vmdk`; do ~/vmdk $A; done
```

This should create output like:

```bash
richard$ for A in `ls *.vmdk`; do ~/vmdk $A; done
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
