+++
title = 'Deploying in a Non-ROOT Context'
date = '2025-09-26T21:03:00+10:00'
draft = false
+++

By default OpenWayback expects to run as the `ROOT` webapp with the `/wayback/` access point (`http://localhost:8080/wayback/`). To deploy under a different context (e.g. `/mycontext`), update `wayback.xml` accordingly.

## Update base URL properties

```xml
<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
  <property name="properties">
    <value>
      wayback.url.scheme=http
      wayback.url.host=localhost
      wayback.url.port=8080
      wayback.url.context=mycontext
      wayback.url.prefix=${wayback.url.scheme}://${wayback.url.host}:${wayback.url.port}/${wayback.url.context}
    </value>
  </property>
</bean>
```

## Adjust the access point

```xml
<bean name="standardaccesspoint" class="org.archive.wayback.webapp.AccessPoint">
  <property name="accessPointPath" value="/wayback/" />
  ...
  <property name="replayPrefix" value="/${wayback.url.context}/wayback/" />
  <property name="queryPrefix" value="/${wayback.url.context}/wayback/" />
  <property name="staticPrefix" value="/${wayback.url.context}/wayback/" />
  ...
  <property name="replayURIPrefix" value="/${wayback.url.context}/wayback/" />
</bean>
```

Restart Tomcat to pick up the new context.
