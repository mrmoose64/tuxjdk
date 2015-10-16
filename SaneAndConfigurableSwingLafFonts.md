# Introduction #

[sane-and-configurable-swing-laf-fonts.diff](https://code.google.com/p/tuxjdk/source/browse/quilt-patches/sane-and-configurable-swing-laf-fonts.diff)

Patch changes default font size of Swing LookAndFeels to widely used 9pt, and adds possibility to configure default font size via swing.properties configuration file.

[Read more about swing.properties file](AddSwingPropertiesConfigurationHierarchy.md)

# Details #

## Default size ##
Oracle JDK and OpenJDK uses 12pt as default font size in Swing. Why 12pt? Because DPI value is not involved in Swing font rendering, and 12pt of Swing produces 9pt fonts on 96 DPI display.

[Read more about OpenJDK DPI issues](FixTypographicalPointSize.md)

This patch changes default font size to 9pt, which is probably the most popular default font size.

## Configure default font size ##
This patch allows the default font size to be read from [swing.properties configuration file](AddSwingPropertiesConfigurationHierarchy.md).

To change default Swing font size to 6, copy the following line to your swing.properties file:
```
swing.defaultFontSize=6
```

In case of any read errors or integer parsing errors, 9 is used as default value.