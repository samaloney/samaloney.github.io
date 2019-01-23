Title: Fix error when plotting from IDL 8.5 
Date: 2016-11-13 22:0
Category: How To
Tags: osx, idl, x11
Authors: Shane Maloney
Summary: Fixing issue IDL 8.5 issue with XQuartz 2.7

Originally from [here](http://blogs.qub.ac.uk/screenshotsfromtheedge/2016/10/25/xquartz-2-7-10-and-libxt-motif-idl/)



Issue when trying to plot from IDL 8.5 running XQuarts 2.7 the follow error occurs

```
Error: attempt to add non-widget child "dsm" to parent "idl" which supports only widgets
```

A workaround is to alter the idl and idlde scripts making sure the DYLD_LIBRARY_PATH environment variable includes the directory /opt/X11/lib/flat_namespace/

So at around line 245 in idl `/Applications/exelis/idl85/bin/idl`
```bash
if [ "$DYLD_LIBRARY_PATH" = "" ]; then
    DYLD_LIBRARY_PATH="/opt/X11/lib/flat_namespace:$BIN_DIR"
    #DYLD_LIBRARY_PATH="$BIN_DIR"
else
    DYLD_LIBRARY_PATH="/opt/X11/lib/flat_namespace:$BIN_DIR:$DYLD_LIBRARY_PATH"
    #DYLD_LIBRARY_PATH="$BIN_DIR:$DYLD_LIBRARY_PATH"
fi
```

and in idlde `/Applications/exelis/idl85/bin/idlde`

```bash
if [ "$APPLICATION" = "idlde" ]; then
    # add bindir for idlde shareable libraries
    #DYLD_LIBRARY_PATH="$BIN_DIR_IDLDE:$DYLD_LIBRARY_PATH"
    DYLD_LIBRARY_PATH="/opt/X11/lib/flat_namespace:$BIN_DIR_IDLDE:$DYLD_LIBRARY_PATH"
    IDL_START_DIR_DARWIN=`pwd`
    export IDL_START_DIR_DARWIN
fi
```

also see XQartz release [notes](https://www.xquartz.org/releases/bare/XQuartz-2.7.10_rc5.html)
