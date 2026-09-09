---
title: "Captive Portal for Linux"
description: "Self-contained Wi-Fi captive portal for Linux: a custom async C# HTTP server plus hostapd/dnsmasq/iptables to gate internet access per client."
tech: [C#, ".NET", Linux, hostapd, dnsmasq, iptables]
date: 2026-01-01 # TODO: set actual date
repo: https://github.com/crackbandicoot-dot/CaptivePortal
---

## Overview

A lightweight captive portal solution that turns a Linux machine into a Wi-Fi
access point with a login page, controlling which connected clients get internet
access. It's made of four pieces working together:

1. A **C# HTTP server**, built from scratch on raw sockets, that serves the login
   page and handles authentication.
2. **hostapd** to create the Wi-Fi access point itself.
3. **dnsmasq** for DHCP and DNS, including redirecting clients to the portal.
4. **iptables**, driven from the C# app, to allow or block a client's traffic
   once they log in or out.

When a client connects and tries to browse, they're redirected to the login page;
after successful authentication their IP/MAC is allowed through the firewall
until they log out.

## Architecture

The C# application is organized around a few key pieces: an async HTTP server
(`CaptivePortalServer`) built on `AsyncHttpServerBase`, a `LoginService` that
checks credentials against a user repository, and an internet-access controller
that translates login/logout events into `iptables` rules. On login, the flow is:

`Client → HTTPServer → LoginService → UserRepository (lookup)` and, if valid,
`LoginService → TrafficController → iptables (allow rules)`; invalid credentials
return a 401 instead.

The project ships a **mock** user repository and a **mock** internet-access
controller by default, so the interfaces (`IUserRepository`,
`IInternetAccesController`) are meant to be swapped for real implementations
(e.g., a database and a production-grade traffic controller) rather than shipped
as-is.

## Design decisions

- Wrote the **HTTP server from scratch** on sockets instead of using
  `HttpListener`/Kestrel, keeping the project dependency-free and giving full
  control over the request lifecycle needed for captive-portal redirects.
- Kept **network plumbing** (hostapd/dnsmasq/iptables) as external tools and
  config files rather than reimplementing AP/DHCP/firewall logic in C#, since
  those are already mature, well-understood Linux components.
- Defined `IUserRepository` and `IInternetAccesController` as interfaces with
  mock implementations, so the authentication and traffic-control logic can be
  swapped out (real database, real firewall backend) without touching the HTTP
  layer.
- Tracked sessions by **IP/MAC** rather than cookies or tokens, matching how a
  captive portal naturally identifies devices on the local network segment.

## What I learned

- Building an asynchronous HTTP server directly on sockets, including parsing
  raw HTTP requests and responses by hand.
- Orchestrating Linux networking tools (`hostapd`, `dnsmasq`, `iptables`) from
  an application layer to control real network access.
- Designing around ARP-cache and MAC-resolution quirks (the controller depends
  on the ARP cache being populated) as a practical constraint of working at the
  network layer instead of purely in application code.
