Asset Bank Dependencies Tools
=============================

Scripts for building external binaries that Asset Bank depends on:
Ghostscript, ImageMagick and ffmpeg.

The current versions of these scripts are *only* for use with Debian 7.0
(Wheezy).

Build Setup
-----------
On any machine that will be used to *build* these tools, run:

    ./setup

this only needs to be run once, not every time you do a build.

Building Ghostscript and ImageMagick
------------------------------------
They are in the same script because ImageMagick depends on Ghostscript.

Download ghostscript-9.06.tar.gz and ImageMagick-6.8.9-0.tar.xz to
~/software/.

Run:

    ./build-ab-gs-im

Building ffmpeg
---------------
Download ffmpeg-1.0.9.tar.bz2 to ~/software/

Run:

    ./build-ab-ffmpeg

Using the built tools
---------------------
Set the following environment variables (either in your user's shell or in
/etc/default/tomcat6):

    LD_LIBRARY_PATH=/usr/local/assetbank/ffmpeg/lib
    PATH="/usr/local/assetbank/{ffmpeg,ghostscript,imagemagick}/bin:$PATH"
    export LD_LIBRARY_PATH PATH

/usr/local/assetbank/{ffmpeg,ghostscript,imagemagick} are symlinks to allow
easy upgrades without Tomcat restarts on Asset Bank servers.

Notes
-----
These scripts build dynamic, not static binaries because static linking
against glibc is not recommended and it is hard to get an autoconf-based
build to link dynamically against glibc and statically against everything
else. Basically, static linking on GNU/Linux seems to be deprecated these
days.

The fact that the binaries are dynamically linked means that they need
various libraries to be installed on the systems that they run on. More
thought needs to be put into how to arrange that, although one quick way
would be to run the 'setup' script on all target hosts; the 'apt-get
build-deps' steps should install all the required libraries (along with many
packages that are not needed, e.g. -dev packages).
