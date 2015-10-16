# Introduction #

[lcd-hrgb-by-default.diff](https://code.google.com/p/tuxjdk/source/browse/quilt-patches/lcd-hrgb-by-default.diff)

Patch changes default antialiasing mode from OFF to LCD\_HRGB.

# Details #

Default value of text antialiasing in Swing is equivalent to OFF. It can be changed by settings rendering hints to Graphics object, but if that part is missed or reset of rendering hints occurred somewhere in the lifecycle of the component, OFF will be used and fonts will look badly in comparison to normal fonts.

This patch makes LCD\_HRGB a default rendering hint, targeting developers who are working in Swing-based tools.

# WARNING #
It is possible that this patch will affect Java printing or off-screen painting, in this case please create an issue, attaching images and reproducable steps.