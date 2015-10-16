# Introduction #

[add-fontconfig-support.diff](https://code.google.com/p/tuxjdk/source/browse/quilt-patches/add-fontconfig-support.diff)

Patch adds fontconfig support to OpenJDK font scaler.

# Details #

[Fontconfig](http://en.wikipedia.org/wiki/Fontconfig) is widely used in Linux distributions to tune font rendering for specific device and user preferences. Number of fontconfig configuration parameters, such as rgba, hinting, hintstyle, antialias and lcdfilter, affect font appearance on the screen, and respected by both Qt and GTK.

But, those rendering parameters are completely ignored by both Oracle Java and OpenJDK.

## Patch internals ##

Patch modified `freetypeScaler.c` to read fontconfig properties and use them as much as possible in glyph image creation.

# Comparison #
Zoom the screenshots to 100% to see the real picture:
  * [Oracle JDK with DejaVu Sans Mono](https://googledrive.com/host/0B68yuEpDuq6wMTBpazBSTnZKWnM/fontconfig/nb-80-96dpi-12pt-dejavu-oracle.png)
  * [TuxJdk with DejaVu Sans Mono](https://googledrive.com/host/0B68yuEpDuq6wMTBpazBSTnZKWnM/fontconfig/nb-80-96dpi-9pt-dejavu-tuxjdk.png)
  * [Oracle JDK with Consolas](https://googledrive.com/host/0B68yuEpDuq6wMTBpazBSTnZKWnM/fontconfig/nb-80-96dpi-12pt-consolas-oracle.png)
  * [TuxJdk with Consolas](https://googledrive.com/host/0B68yuEpDuq6wMTBpazBSTnZKWnM/fontconfig/nb-80-96dpi-9pt-consolas-tuxjdk.png)