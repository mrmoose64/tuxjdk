# Introduction #

[add-swing-properties-configuration-hierarchy.diff](https://code.google.com/p/tuxjdk/source/browse/quilt-patches/add-swing-properties-configuration-hierarchy.diff)

Patch adds a possibility to add system-wide and user-specific swing.properties configuration file.

# Details #

JDK uses `swing.properties` configuration file when initializing Swing properties. But that file is read from `$JAVA_HOME/lib/swing.properties` file. That becomes very inconvenient if you're using more than one java on the system.

This patch utilizes the idea of standard Linux configuration hierarchy for swing.properties file.

When requested for some configuration parameters, following configuration files priority takes place:
  * `{user.home}/.config/java/swing.properties`
  * `/etc/java/swing.properties`
  * `{java.home}/lib/swing.properties` for backward compatibility.