# Introduction #

[fix-typographical-point-size.diff](https://code.google.com/p/tuxjdk/source/browse/quilt-patches/fix-typographical-point-size.diff)

Patch fixes pt size in Swing to proper typographical point size.

# Details #

There is a convention of [typographical point](http://en.wikipedia.org/wiki/Point_(typography)) in UI design, which is universally sized to 1/72 of an inch. That convention is respected by (probably) all existing UI toolkits, including GTK, Qt and CSS.

Swing, however, does not respect that convention, in swing `1pt == 1px`. It becomes extremely obvious on displays with high DPI, like 13" FullHD laptops.

## Patch internals ##

When making a call to [freetype2](http://en.wikipedia.org/wiki/FreeType), OpenJDK FreetypeScaler used hardcoded value of 72 as DPI value.

AWS has a concept of [normalizing transform](http://docs.oracle.com/javase/8/docs/api/java/awt/GraphicsConfiguration.html#getNormalizingTransform--), that can be used for DPI-independent UI design and to scale typographical point size to pixel size. That method is used somewhere in PangoFonts class, but nowhere else.

This patch utilizes normalizing transform functionality in FreetypeScaler, to request proper pixel sizes from freetype2.

# Comparison #

All screenshots are made on average 96 DPI display, under KDE with default font size of 9pt:
| KWrite, 9pt | ![https://googledrive.com/host/0B68yuEpDuq6wWlFCN2NHTHl1aXM/kwrite-9pt.png](https://googledrive.com/host/0B68yuEpDuq6wWlFCN2NHTHl1aXM/kwrite-9pt.png) |
|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------|
| Oracle JDK, 9pt | ![https://googledrive.com/host/0B68yuEpDuq6wWlFCN2NHTHl1aXM/jdk-oracle-9pt.png](https://googledrive.com/host/0B68yuEpDuq6wWlFCN2NHTHl1aXM/jdk-oracle-9pt.png) |
| Oracle JDK, 12pt | ![https://googledrive.com/host/0B68yuEpDuq6wWlFCN2NHTHl1aXM/jdk-oracle-12pt.png](https://googledrive.com/host/0B68yuEpDuq6wWlFCN2NHTHl1aXM/jdk-oracle-12pt.png) |
| TuxJdk, 9pt | ![https://googledrive.com/host/0B68yuEpDuq6wWlFCN2NHTHl1aXM/jdk-tuxjdk-9pt.png](https://googledrive.com/host/0B68yuEpDuq6wWlFCN2NHTHl1aXM/jdk-tuxjdk-9pt.png) |

Zooming in shows that TuxJdk provides almost pixel-perfect match with Qt-based font rendering.