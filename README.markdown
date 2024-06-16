---
labels:
- 'Stage-Alpha'
summary: s2s to I2P hidden services
...

Introduction
============

This plugin allows Prosody to connect to other servers that are running
as a I2P hidden service. Running Prosody on a hidden service works
without this module, this module is only necessary to allow Prosody to
federate to hidden XMPP servers.

fork from mod_onions

Usage
=====

This module depends on the bit32 Lua library.

To create a hidden service that can federate with other hidden XMPP
servers, first add a hidden serivce to Tor. It should listen on port
5269 and optionally also on 5222 (if c2s connections to the hidden
service should be allowed).

Use the hostname that Tor gives with a virtualhost:

    VirtualHost "somethingbase32.i2p"
        modules_enabled = { "i2p" };

Configuration
=============

  Name                   Description                                           Type      Default value
  ---------------------- ----------------------------------------------------- --------- ---------------
  i2p\_socks5\_host   the host to connect to for Tor's SOCKS5 proxy         string    "127.0.0.1"
  i2p\_socks5\_port   the port to connect to for Tor's SOCKS5 proxy         integer   9050
  i2p\_only           forbid all connection attempts to non-onion servers   boolean   false
  i2p\_s2s\_all       pass all s2s connections through Tor                  boolean   false
  i2p\_map            override the address for a host                       table     {}

By setting `i2p_map`, it is possible to override the address used to
connect to a given host with the address of a hidden service. The
configuration of `i2p_map` works as follows:

    onions_map = {
        ["jabber.calyxinstitute.org"] = "ijeeynrc6x2uy5ob.i2p";
    }

or, to also specify a port:

    onions_map = {
        ["jabber.calyxinstitute.org"] = { host = "ijeeynrc6x2uy5ob.i2p", port = 5269 };
    }

Compatibility
=============

  ----- --------------

  ----- --------------

Notes
=====

-   `i2p_s2s_all` does not look up SRV records first. Therefore it
    will fail for many servers.
-   mod\_i2p currently does not support connecting to `.i2p`
    entries in SRV records.

Security considerations
=======================

-   Running a hidden service on a server together with a normal server
    might expose the hidden service.
-   A hidden service that wants to remain hidden should either disallow
    s2s to non-hidden servers or pass all s2s traffic through Tor
    (setting either `i2p_only` or `i2p_s2s_all`).
