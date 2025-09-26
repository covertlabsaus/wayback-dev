+++
title = 'Creating a WAR Overlay'
date = '2025-09-26T21:03:00+10:00'
draft = false
+++

> Requires familiarity with [Apache Maven](https://maven.apache.org/).

## What is a WAR overlay?

A [WAR file](https://en.wikipedia.org/wiki/WAR_(file_format)) (Web Application Archive) is a ZIP archive with a well-defined layout. It contains everything needed to deploy a Java web application on a servlet container such as [Apache Tomcat](https://tomcat.apache.org/).

A WAR overlay is a Maven project that depends on another project's WAR artifact. During the build, Maven explodes the dependency WAR, applies your overlay files on top (adding new files or replacing existing ones), and packages the result into a new WAR.

## Why overlay OpenWayback?

OpenWayback's configuration lives inside its WAR (`WEB-INF/wayback.xml` and `WEB-INF/classes/WaybackUI.properties`). You could edit these files after deployment, but they may be overwritten during upgrades. Maintaining a fork is another option but makes it harder to track upstream changes.

Using an overlay keeps your customisations isolated: only your overrides live in the overlay project, while the base OpenWayback code remains untouched.

## Maven setup

Create a Maven webapp project and depend on the OpenWayback artifacts:

```xml
<dependency>
  <groupId>org.netpreserve.openwayback</groupId>
  <artifactId>openwayback-core</artifactId>
  <version>2.0.0</version>
</dependency>
<dependency>
  <groupId>org.netpreserve.openwayback</groupId>
  <artifactId>openwayback-webapp</artifactId>
  <version>2.0.0</version>
  <type>war</type>
  <scope>runtime</scope>
</dependency>
```

Configure the WAR plugin to overlay the webapp:

```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-war-plugin</artifactId>
  <version>2.1.1</version>
  <configuration>
    <overlays>
      <overlay>
        <groupId>org.netpreserve.openwayback</groupId>
        <artifactId>openwayback-webapp</artifactId>
      </overlay>
    </overlays>
  </configuration>
</plugin>
```

Add files under the same paths to replace the originals, e.g.:

- `src/main/webapp/WEB-INF/wayback.xml`
- `src/main/resources/WaybackUI.properties`

You can also add new JSPs or Java classes. Wire custom components in via `wayback.xml`. You cannot overlay classes from `openwayback-core`, but you can provide replacements and reference them in configuration.

Build the overlay:

```bash
mvn package
```

The resulting WAR contains your configuration and is ready to deploy on Tomcat. Command-line tools (CDX builders, etc.) still require the standard OpenWayback distribution.

## Sample project

See <https://github.com/iipc/openwayback-sample-overlay> for a working example.

## IDE tips

[Eclipse](https://www.eclipse.org/) with the Java EE bundle has Maven, Git, and web tooling out of the box, making it convenient for overlay work. Other IDEs work too as long as they support Maven webapps.

## Environment-specific configuration

To separate configuration from the packaged WAR, leverage Spring's `PropertyPlaceholderConfigurer` in `wayback.xml`:

```xml
<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
  <property name="location" value="file:${user.home}/wayback.properties" />
</bean>
```

Store deployment-specific values in `~/wayback.properties` and reference them later in `wayback.xml`:

```xml
<property name="resourceStore">
  <bean class="org.archive.wayback.resourcestore.LocationDBResourceStore">
    <property name="db">
      <bean class="org.archive.wayback.resourcestore.locationdb.FlatFileResourceFileLocationDB">
        <property name="path" value="${wayback.archivespath}" />
      </bean>
    </property>
  </bean>
</property>
```

Include a template properties file in your overlay so new deployments know which keys to populate.
