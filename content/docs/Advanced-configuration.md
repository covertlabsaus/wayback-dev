+++
title = 'Advanced Configuration'
date = '2025-09-26T21:03:00+10:00'
draft = false
+++

This section covers configuration tasks that go beyond the initial setup. If you are just getting started, read the [basic configuration guide](/docs/How-to-configure) first.

## Index tuning

Learn how OpenWayback maps incoming URLs to archived captures and how to optimise search behaviour:

- [Configuring ZipNumCluster](/docs/Configuring-ZipNumCluster)

## Custom access points

OpenWayback can expose the same collection through multiple access points (proxy mode, Memento, custom replay UIs, etc.). The default `AccessPoint` handler can be customised with different renderers and parsers, or replaced entirely via a [WAR overlay](/docs/Creating-a-WAR-overlay).

- [AccessPoint overview](/docs/AccessPoint-overview)
- [Creating a WAR overlay](/docs/Creating-a-WAR-overlay)

## Other scenarios

- [Deploying OpenWayback in a non-ROOT context](/docs/Deploying-OpenWayback-in-non-ROOT-Context)
