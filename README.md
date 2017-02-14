OpenArenaPandora
================

OpenArena version for OpenPandora, featuring ARM support and GLES renderer.

The current state is playable without any major problems and good performances. 
Only limits are flares that requires depth buffer reading, and QVM that is interpreted (no JIT on ARM)

OpenPandora
===========

This version is aimed at OpenPandra, so get:
 * ARM compatibilty (using -DARM)
 * Some NEON optimized code (using -DNEON)
 * GLES renderer (using -DHAVE_GLES)
 * Toggle Crouch function (using -DCROUCH), disabled by default and with a new option in cfg
 * OpenPandora support of course (using -DPANDORA), for screen resolution mainly.
 * ODROID support (build with make ODROID=1), mostly like Pandora, but without the control and resolution hack

There are some hard-coded value in the Makefile, located at the beggining to force "OpenPandora version" (COMPILE_PLATFORM=pandora and COMPILE_ARCH=arm).
 
For more info on the OpenPandora go here: http://boards.openpandora.org/
Specific thread for Jedi Academy on the OpenPandora here: http://boards.openpandora.org/index.php/topic/13695-open-arena/

Amlogic S905 Mainline Linux
===========================

Build Mainline linux with :
- Mali module : https://github.com/superna9999/meson_gx_mali_450
- Mali patch : https://github.com/superna9999/meson_gx_mali_450/tree/DX910-SW-99002-r6p1-01rel0_meson_gx/patches_linux-4.10
- armsoc video driver for X11 : https://github.com/superna9999/xf86-video-armsoc
- Mali libraries : http://openlinux.amlogic.com:8000/download/ARM/filesystem/arm-buildroot-2016-08-18-5aaca1b35f.tar.gz

Get libMali.so from :
```
buildroot/package/opengl/src/lib/arm64/r6p1/m450-X/
```

in the buildroot tarball.

Where /usr/lib/mali/ is where the symlinks to libMali.so are and ldconfig was configured to fetch the EGL libraries by adding a file into :
```
/etc/ld.so.conf.d/mali.conf
```

containing :
```
/usr/lib/mali
```

And running :
```
# ldconfig
```

Mali libraries should have the following symlinks :
```
libEGL.so -> libEGL.so.1
libEGL.so.1 -> libEGL.so.1.4
libEGL.so.1.4 -> libMali.so
libGLESv1_CM.so -> libGLESv1_CM.so.1
libGLESv1_CM.so.1 -> libGLESv1_CM.so.1.1
libGLESv1_CM.so.1.1 -> libMali.so
libGLESv2.so -> libGLESv2.so.2
libGLESv2.so.2 -> libGLESv2.so.2.0
libGLESv2.so.2.0 -> libMali.so
libMali.so
```

Then :
```
LDFLAGS=-L/usr/lib/mali/ make ODROID=1
```

to build the OpenArena ioquake3 engine.
