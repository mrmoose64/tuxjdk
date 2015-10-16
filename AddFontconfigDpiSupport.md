# Introduction #

Patch fixes DPI-related font rendering issues in OpenJDK and adds support for fontconfig.


# How it supposed to work #

Font design relies on [typographical point](http://en.wikipedia.org/wiki/Point_(typography)) for measurement, which is 1/72 of an inch. Those points are used in HTML and freetype2 (which means that Qt, GTK and most of existing UI toolkits are using it too).

To render fonts properly on any device, point size must be converted to pixel size for concrete device. DPI is used for that calculation. Qt, for example, calculates DPI getting physical dimensions and resolution of a display from Xlib, and provides that value to freetype2.

On modern LCD displays, freetype2 uses subpixel antialiasing and hinting techniques to make fonts look really good. Both of those techniques rely on accurate DPI values.

[Fontconfig](http://en.wikipedia.org/wiki/Fontconfig) usually provides parameters for hinting type, rendering mode and LCD filtering, and is used by both Qt and GTK before making request to freetype2. Support of fontconfig usually allows all UI toolkits to render fonts uniformly, even if UI toolkits are unrelated.

# How DPI it is done in Java #

Java FreetypeFontScaler uses hardcoded value of 72 DPI when working with freetype. As a result, font geometry, subpixel antialiasing and hinting are ruined.
In addition, in Swing application one typographical point is equal to one device pixel, breaking DPI-independent UI design possibilities. Looks like the problem is well known to Swing developers, because default font size in Swing is 12pt, which corresponds to 9pt in Qt/GTK applications on 96 DPI display.

There are 2 methods in Java that should handle this correctly, but for some reason they are used only in PangoFonts class:
  * [GraphicsConfiguration.getDefaultTransform()](http://docs.oracle.com/javase/8/docs/api/java/awt/GraphicsConfiguration.html#getDefaultTransform--)
  * [GraphicsConfiguration.getNormalizingTransform()](http://docs.oracle.com/javase/8/docs/api/java/awt/GraphicsConfiguration.html#getNormalizingTransform--)

# How fontconfig support is done in Java #

It's not. Java has 4 modes of rendering, switched with _awt.useSystemAAFontSettings_ property, and that's it.

# What this patch does #

  * DPI is read from fontconfig;
  * DPI is read from Xlib;
  * both Xlib and fontconfig report default DPI, which is relatively small (75 for fontconfig, and probably either 72 or 96 for Xlib). So higher value is chosen for resulting DPI;
  * fontconfig properties are read and corresponding freetype2 parameters are set;
  * freetype2 is set with all the parameters;
  * font requested with DPI calculated at step 3.

# Comparison #
  * [NetBeans on Oracle Java 8 and 14 points font, 13" FullHD laptop (zoom to 100% to see the uglyness)](https://googledrive.com/host/0B68yuEpDuq6wMTBpazBSTnZKWnM/netbeans_oracle_java_8.png)
  * [NetBeans on tuxjdk 8 and 6 points font, 13" FullHD laptop (zoom to 100% to see the beauty)](https://googledrive.com/host/0B68yuEpDuq6wMTBpazBSTnZKWnM/netbeans_tuxjdk_8.png)

# Alternative solution for DPI issue #

It is possible to replace [FT\_Set\_Char\_Size](https://code.google.com/p/tuxjdk/source/browse/quilt-patches/add-fontconfig-dpi-support.diff#253) invocation with [FT\_Set\_Pixel\_Sizes](http://www.freetype.org/freetype2/docs/reference/ft2-base_interface.html#FT_Set_Pixel_Sizes) invocation, and remove multiplication of ptsz in _freetypeScaler.c_ source file, and the result will be similar to current version, but that conceptually wrong, because it breaks the idea of typographical point abstraction, making what java calls a _pt_ will actually be a _px_.