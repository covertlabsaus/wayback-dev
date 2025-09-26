+++
title = "Tune OpenWayback Performance and Caching"
description = "Adjust caching and thread pools to improve OpenWayback responsiveness."
draft = false
+++

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
      "mainEntity": [{
    "@type": "Question",
    "@id": "https://wayback.dev/faq/openwayback-performance-tuning",
    "name": "How do I tune OpenWayback performance and caching?",
    "acceptedAnswer": {
      "@type": "Answer",
      "text": "Increase Tomcat connector threads, enable gzip of CDX responses, and configure resource caching (ehcache) to store frequently accessed replay content."
    }
  }]
}
</script>

High-traffic archives benefit from careful tuning.

## Tomcat connector

```xml
<Connector port="8080" protocol="HTTP/1.1"
           maxThreads="400"
           connectionTimeout="20000"
           enableLookups="false" />
```

## Ehcache snippet (`WEB-INF/ehcache.xml`)

```xml
<cache name="ReplayCache"
       maxEntriesLocalHeap="5000"
       timeToLiveSeconds="3600" />
```

## Compress responses

```xml
<Valve className="org.apache.catalina.valves.RemoteIpValve" />
<Connector ... compression="on" compressionMinSize="1024" />
```

## Diagram

```mermaid
flowchart LR
    A[Tomcat threads] --> B[Ehcache]
    B --> C[Faster replay]
```

Monitor JVM memory and adjust heap sizes to avoid GC pauses while caching.
