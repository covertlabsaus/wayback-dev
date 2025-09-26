+++
title = 'How to Install'
date = '2025-09-26T21:03:00+10:00'
draft = false
+++

## System requirements

- **Java:** Oracle/OpenJDK 7 or newer
- **Tomcat:** Apache Tomcat 6.0 or newer

Linux/UNIX environments are recommended.

## Download OpenWayback

OpenWayback ships as a WAR file. Grab the binary distribution from [Maven Central](https://search.maven.org/search?q=g:org.netpreserve.openwayback) or build from source.

### Binary distribution

```bash
tar -xzvf openwayback-dist-<version>.tar.gz
```

The archive contains `bin/`, `lib/`, and `openwayback-<version>.war`.

### Build from source

1. Clone the repository:

   ```bash
   git clone https://github.com/iipc/openwayback.git
   ```

   Or download a ZIP snapshot:

   ```bash
   wget https://github.com/iipc/openwayback/archive/master.zip -O openwayback.zip
   unzip openwayback.zip
   mv openwayback-master openwayback
   ```

2. Build with Maven 2+:

   ```bash
   cd openwayback
   mvn package
   ```

   Add `-Dmaven.test.skip=true` if unit tests fail on your platform.

3. Locate artifacts:

   - Full distribution: `openwayback/dist/target/openwayback-dist-<version>.tar.gz`
   - WAR only: `openwayback/wayback-webapp/target/openwayback-webapp-<version>.war`

## Install into Tomcat

1. Rename the WAR to `ROOT.war`.
2. Copy it to `$CATALINA_HOME/webapps/`.
3. Wait for Tomcat to explode the archive.
4. Customise `WEB-INF/wayback.xml` (see [How to configure](/docs/How-to-configure)).
5. Restart Tomcat.

To deploy under a different context, follow [Deploying in a Non-ROOT Context](/docs/Deploying-OpenWayback-in-non-ROOT-Context).
