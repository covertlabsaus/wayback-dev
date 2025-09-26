+++
title = 'AccessPoint Overview'
date = '2025-09-26T21:03:00+10:00'
draft = false
+++

OpenWayback routes traffic through **access points**. You can run multiple access points to expose the same collection under different URL prefixes or ports (configure Tomcat accordingly).

## Required components

Each access point must define implementations for:

- `collection` — the `WaybackCollection` being exposed.
- `query` — renders search results and captures user input.
- `replay` — chooses the correct `ReplayRenderer` per request.
- `uriConverter` — builds replay URLs for matched records (see replay modes).
- `parser` — turns incoming HTTP requests into `WaybackRequest` objects.

## Request mapping

- `accessPointPath` — URL prefix, e.g. `http://host/context/`.
- `internalPort` — optional explicit port if it is not part of the path.

## Optional settings

Define at least one of `replayPrefix`, `queryPrefix`, or `staticPrefix`. Additional options include:

- `exception` — custom error page handler.
- `configs` — key/value pairs exposed to JSP templates.
- `exclusionFactory` — controls which documents are visible.
- `authentication` — enforces user access rules.
- `replayPrefix` — base URL for replay requests (defaults to `queryPrefix`, then `staticPrefix`).
- `queryPrefix` — base URL for query requests (defaults to `staticPrefix`, then `replayPrefix`).
- `staticPrefix` — base URL for shared UI assets (defaults to `queryPrefix`, then `replayPrefix`).
- `livewebPrefix` — points to an access point configured for live web fetching.
- `locale` — forces a locale regardless of browser preferences.
- `exactHostMatch` — when true, restrict results to the exact hostname (case insensitive).
- `exactSchemeMatch` — when true, require the same scheme (defaults to `true`).

Access points let you tailor access for different audiences. For instance, expose both proxy and archival URL modes simultaneously, or restrict external users to robots.txt compliant content while internal users see everything.

See `wayback.xml` inside `wayback.war` for complete configuration samples.
