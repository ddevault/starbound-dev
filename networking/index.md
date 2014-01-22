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

* **pstr**: A length prefixed string, with a VLQ specifying the length.
* **[u]int##**: An integer, in big-endian order. "u" indicates unsiged and ## is the length, in bits. Examples: uint8, int32, etc.
* **[s]VLQ**: [Variable length quantity](https://en.wikipedia.org/wiki/Variable-length_quantity). "s" indicates signed.

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
            <td>sVLQ</td>
            <td>Payload length</td>
            <td>The length of the payload, in bytes. If it is negative, this indicates the payload is compressed using zlib.</td>
        </tr>
        <tr>
            <td>Varies</td>
            <td>Payload</td>
            <td>Varies</td>
        </tr>
    </tbody>
</table>

## Packets


### 0x01: Protocol Version {% include bidirectional.md %}

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x02: Connection Response {% include client-to-server.md %}

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x03: Server Disconnect {% include server-to-client.md %}

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x04: Handshake Challenge

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x05:Chat Received

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x06: Universe Time Update

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x07: Client Connect

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x08: Client Disconnect

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x09: Handshake Response

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x0a: Warp Command

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x0b: Chat Sent

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x0c: Client Context Update

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x0d: World Start

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x0e: World Stop

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x0f: Tile Array Update

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x10: Tile Update

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x11: Tile Liquid Update

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x12: Tile Damage Update

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x13: Tile Modification Failure

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x14: Give Item

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x15: Unknown

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x16: Swap in Container Result

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x17: Environment Update

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x18: Entity Interact Result

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x19: Modify Tile List

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x1a: Damage Tile

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x1b: Damage Tile Group

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x1c: Request Drop

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x1d: Spawn Entity

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x1e: Entity Interact

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x1f: Connect Wire

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x20: Disconnect All Wires

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x21: Open Container

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x22: Close Container

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x23: Swap in Container

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x24: Item Apply in Container

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x25: Start Crafting in Container

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x26: Stop Crafting in Container

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x27: Burn Container

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x28: Clear Container

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x29: World Update

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x2a: Entity Create

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x2b: Entity Update

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x2c: Entity Destroy

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x2d: Damage Notification

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x2e: Status Effect Request

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x2f: Update World Properties

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>

### 0x30: Heartbeat

This packet does blah blah blah.

<table class="table packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">0x01</td></tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Example</td>
            <td>Lorem ipsum dolor sit amet, consectetur adipiscing elit</td>
        </tr>
    </tbody>
</table>