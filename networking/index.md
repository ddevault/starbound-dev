---
layout: base
title: Starbound Networking
---

# Starbound Networking

The Starbound networking protocol is based on TCP. The current protocol version is 1234.

## Resources

There are several related pages that you might want to browse:

* <span class="text-danger">Establishing a connection</span>

## Data Types

<!--
    This section describes terminology that will see use for all packets below. If a new data
    type is introduced, then document it here.
-->

Note that I don't actually know the data types, I'm filling this with bullshit for others to fix later.

* **string**: UTF-8, prefixed with a uint8 indicating length.
* **[u]int##**: An integer, in big-endian order. "u" indicates unsiged and ## is the length, in bits. Examples: uint8, int32, etc.
* **[u]vlq**: [Variable length quantity](https://en.wikipedia.org/wiki/Variable-length_quantity). "u" indicates unsigned.

## Base Packet

Each packet is wrapped in this basic package:

<table class="table">
    <thead>
        <tr>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>uint8</td>
            <td>Packet ID</td>
            <td>The packet identifier. The payload format changes based on this value.</td>
        </tr>
        <tr>
            <td>vrq</td>
            <td>Payload length</td>
            <td>The length of the payload, in bytes.</td>
        </tr>
        <tr>
            <td>Varies</td>
            <td>Payload</td>
            <td>Varies</td>
        </tr>
    </tbody>
</table>

## Packets

TODO
