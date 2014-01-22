---
layout: base
title: Starbound Networking
---

# Starbound Networking

The Starbound networking protocol is based on TCP, and uses port 21025 by default. The current protocol version (Angry Koala) is 0x274.

## Resources

There are several related pages that you might want to browse:

* [Data types](/networking/data-types.html)
* [Logging on to a server](/todo)

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

### 0x01: Protocol Version {% include server-to-client.md %}

This packet is the first packet sent. It contains the server version.

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
        <tr><td rowspan="2">0x01</td></tr>
        <tr>
            <td>VLQ</td>
            <td>Protocol version number</td>
            <td>A number that provides the protocol version. Changes with each release.</td>
        </tr>
    </tbody>
</table>

### 0x02: Connection Response {% include server-to-client.md %}

This packet tells the client whether their connection attempt is successful or if they have been rejected. It is the final packet sent in the handshake process.

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
        <tr><td rowspan="4">0x02</td></tr>
        <tr>
            <td>bool</td>
            <td>Success</td>
            <td>Whether or not the connection was successful.</td>
        </tr>
        <tr>
            <td>VLQ</td>
            <td>Client ID</td>
            <td>The identifier for the connecting client.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Rejection reason</td>
            <td>A string signifying why the user was rejected. This will be present even if the connection was successful, but will simply have a length prefix of 0.</td>
        </tr>
    </tbody>
</table>

### 0x03: Server Disconnect {% include server-to-client.md %}

This packet is used to notify the client of a disconnect.

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
        <tr><td rowspan="1">0x06</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x04: Handshake Challenge {% include server-to-client.md %}

This packet provides a salt and round count for password verification. It is followed by Handshake Response.

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
        <tr><td rowspan="4">0x04</td></tr>
        <tr>
            <td>string</td>
            <td>Claim Message</td>
            <td></td>
        </tr>
        <tr>
            <td>string</td>
            <td>Salt</td>
            <td>The salt to use when hashing the password.</td>
        </tr>
        <tr>
            <td>int32</td>
            <td>Round count</td>
            <td>The number of hashing rounds to perform on the password. Default: 5000</td>
        </tr>
    </tbody>
</table>

### 0x05:Chat Received {% include server-to-client.md %}

This packet is sent to a client with a chat message.

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
        <tr><td rowspan="6">0x05</td></tr>
        <tr>
            <td>uint8</td>
            <td>Channel</td>
            <td>The chat channel.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>World</td>
            <td>The world the sender is on.</td>
        </tr>
        <tr>
            <td>uint32</td>
            <td>Client ID</td>
            <td>The sender's client ID.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Name</td>
            <td>The sender's name in game.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Name</td>
            <td>The chat message.</td>
        </tr>
    </tbody>
</table>

### 0x06: Universe Time Update {% include server-to-client.md %}

This packet is sent from the server to update the current time.

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
        <tr><td rowspan="1">0x06</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x07: Client Connect {% include client-to-server.md %}

**Compressed**

This packet is sent in the handshake process immediately after 0x01, Protocol Version. It contains all relevant data about the connecting player.

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
        <tr><td rowspan="9">0x07</td></tr>
        <tr>
            <td>uint8[]</td>
            <td>Asset Digest</td>
            <td>The digest of the asset folder. Used to detect if any mods are active.</td>
        </tr>
        <tr>
            <td>Variant</td>
            <td>Claim</td>
            <td>Unknown, presumably relates to key exchange. Seems to be unused in the current version.</td>
        </tr>
        <tr>
            <td>bool</td>
            <td>UUID flag</td>
            <td>If true, a UUID follows. If false, it does not.</td>
        </tr>
        <tr>
            <td>uint8[16]</td>
            <td>UUID</td>
            <td>The UUID is read sequentially as the hex dump of the bytes read. This only exists if the UUID flag is true.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Player name</td>
            <td>The name of the player.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Species</td>
            <td>The species of the player.</td>
        </tr>
        <tr>
            <td>uint8[]</td>
            <td>Shipworld</td>
            <td>The player's .shipworld file, without the file header information.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Account</td>
            <td>The player's account name. Currently unused.</td>
        </tr>
    </tbody>
</table>

### 0x08: Client Disconnect {% include client-to-server.md %}

This packet is sent when the client disconnects.

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
        <tr><td rowspan="1">0x08</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x09: Handshake Response {% include client-to-server.md %}

This packet is the response to 0x04: Handshake Challenge,

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
        <tr><td rowspan="3">0x09</td></tr>
        <tr>
            <td>string</td>
            <td>Claim Response</td>
            <td>A response to the claim message. Currently unused.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Password hash</td>
            <td>The hash of the entered password, salted with the information in 0x04.</td>
        </tr>
    </tbody>
</table>

### 0x0A: Warp Command {% include bidirectional.md %}

This packet is sent when the player warps/is warped to a planet or ship.

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
        <tr><td rowspan="4">0x0A</td></tr>
        <tr>
            <td>uint32</td>
            <td>Warp type</td>
            <td>The type of warp being done. TODO: The enum of the four types.</td>
        </tr>
        <tr>
            <td>world_coordinate</td>
            <td>World coordinates</td>
            <td>The world coordinates being warped to. Currently only used in Warp to Spawn.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Player name</td>
            <td>The name of the player being warped to. Currently only used to beam to other ships.</td>
        </tr>
    </tbody>
</table>

### 0x0B: Chat Sent {% include client-to-server.md %}

This packet is sent from the client whenever a message is sent in the chat window.

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
        <tr><td rowspan="3">0x0B</td></tr>
        <tr>
            <td>string</td>
            <td>Message</td>
            <td>The chat message being sent.</td>
        </tr>
        <tr>
            <td>unit8</td>
            <td>Channel</td>
            <td>The current chat channel.</td>
        </tr>
    </tbody>
</table>

### 0x0C: Client Context Update

This packet has yet to be fully understood.

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
        <tr><td rowspan="2">0x0C</td></tr>
        <tr>
            <td>uint8[]</td>
            <td>Client Context Data</td>
            <td>Myriad formats.</td>
        </tr>
    </tbody>
</table>

### 0x0D: World Start {% include server-to-client.md %}

This packet is sent to the client when a world thread has been started on the server.

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
        <tr><td rowspan="6">0x0D</td></tr>
        <tr>
            <td>uint8[]</td>
            <td>Planet</td>
            <td>The world data. WRLDB format with headers stripped.</td>
        </tr>
        <tr>
            <td>uint8[]</td>
            <td>World structure</td>
            <td>The world structure. TODO.</td>
        </tr>
        <tr>
            <td>uint8[]</td>
            <td>Sky</td>
            <td>Data relating to the sky. TODO.</td>
        </tr>
        <tr>
            <td>uint8[]</td>
            <td>Server Weather</td>
            <td>Data relating to the server weather. TODO.</td>
        </tr>
        <tr>
            <td>float</td>
            <td>Spawn X</td>
            <td>The spawn coordinates for the planet. Currently locked in server-side.</td>
        </tr>
        <tr>
            <td>float</td>
            <td>Spawn Y</td>
            <td>The spawn coordinates for the planet. Currently locked in server-side.</td>
        </tr>
    </tbody>
</table>

### 0x0E: World Stop {% include server-to-client.md %}

Called when a world thread is stopped.

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
        <tr><td rowspan="2">0x0E</td></tr>
        <tr>
            <td>string</td>
            <td>Status</td>
            <td>The status of the world.</td>
        </tr>
    </tbody>
</table>

### 0x0F: Tile Array Update

Called when an array of tiles has its properties updated.

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
        <tr><td rowspan="1">0x0F</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x10: Tile Update

This packet is called when a tile is updated.

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
        <tr><td rowspan="1">0x10</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x11: Tile Liquid Update

This packet is sent when the liquid on a tile has changed position.

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
        <tr><td rowspan="1">0x11</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x12: Tile Damage Update

This packet is sent when a tile is damaged.

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
        <tr><td rowspan="1">0x12</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x13: Tile Modification Failure {%include server-to-client.md %}

This packet is sent when a packet cannot successfully be modified.

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
        <tr><td rowspan="1">0x13</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x14: Give Item {% include server-to-client.md %}

This packet attempts to give an item to a player. If the player's inventory is full, it will drop on the ground next to them.

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
        <tr><td rowspan="4">0x14</td></tr>
        <tr>
            <td>string</td>
            <td>Item name</td>
            <td>The name of the item, from the ingame assets. Case sensitive.</td>
        </tr>
        <tr>
            <td>VLQ</td>
            <td>Count</td>
            <td>The number of items to give to the player. If greater than stackSize, it will give up to that amount.</td>
        </tr>
        <tr>
            <td>Variant</td>
            <td>Item properties</td>
            <td>A variant giving the item properties; for non-randomly generated/non-modified items it is just the String variant type.</td>
        </tr>
    </tbody>
</table>

### 0x15: Unknown

The purpose of this packet is totally unknown. It shows up infrequently/never.

### 0x16: Swap in Container Result

This packet is sent whenever two items are swapped in an open container.

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
        <tr><td rowspan="1">0x16</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x17: Environment Update {% include server-to-client.md %}

This packet is sent on an environment update.

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
        <tr><td rowspan="1">0x17</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x18: Entity Interact Result {% include server-to-client.md %}

This packet contains the results of an entity interaction.

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
        <tr><td rowspan="4">0x18</td></tr>
        <tr>
            <td>uint8</td>
            <td>Client ID</td>
            <td>The client attempting to interact.</td>
        </tr>
        <tr>
            <td>int8</td>
            <td>Enitity ID</td>
            <td>The ID of the entity that client is trying to interact with.</td>
        </tr>
        <tr>
            <td>Variant</td>
            <td>Results</td>
            <td>A variant describing the results of the interaction.</td>
        </tr>
    </tbody>
</table>

### 0x19: Modify Tile List {% include client-to-server.md %}

This packet contains a list of tiles and modifications to them.

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
        <tr><td rowspan="1">0x19</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x1A: Damage Tile {% include client-to-server.md %}

This packet updates a tile's damage

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
        <tr><td rowspan="1">0x1A</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x1B: Damage Tile Group {% include client-to-server.md %}

This packet updates an entire tile group's damage.

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
        <tr><td rowspan="1">0x1B</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x1C: Request Drop {% include server-to-client.md %}

This packet requests an item drop (from a player's inventory?)

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
        <tr><td rowspan="1">0x1C</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x1D: Spawn Entity {% include client-to-server.md %}

This packet requests that the server spawn an entity.

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
        <tr><td rowspan="1">0x1D</td><td colspan=3 align='center'>TODO</td></tr>
</table>

### 0x1E: Entity Interact {% include client-to-server.md %}

This packet is sent when a client attempts to interact with an entity.

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
        <tr><td rowspan="1">0x1E</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x1F: Connect Wire

This packet connects a wire.

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
        <tr><td rowspan="1">0x1F</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x20: Disconnect All Wires

This packet disconnects all wires.

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
        <tr><td rowspan="1">0x20</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x21: Open Container {% include client-to-server.md %}

This packet opens a container.

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
        <tr><td rowspan="1">0x21</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x22: Close Container {% include client-to-server.md %}

This packet closes a container.

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
        <tr><td rowspan="1">0x22</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x23: Swap in Container {% include client-to-server.md %}

This packet swaps an item in a container.

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
        <tr><td rowspan="1">0x23</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x24: Item Apply in Container {% include client-to-server.md %}

This packet applies an item to another item in a container.

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
        <tr><td rowspan="1">0x24</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x25: Start Crafting in Container {% include client-to-server.md %}

This packet initiates crafting on an item in a container (Used in pixel compressors and the like?)

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
        <tr><td rowspan="1">0x25</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x26: Stop Crafting in Container {% include client-to-server.md %}

This packet stops crafting on an item in a container

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
        <tr><td rowspan="1">0x26</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x27: Burn Container {% include client-to-server.md %}

This packet burns a container.

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
        <tr><td rowspan="1">0x27</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x28: Clear Container {% include client-to-server.md %}

This packet clears a container.

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
        <tr><td rowspan="1">0x28</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x29: World Update {% include server-to-client.md %}

This packet contains a world update

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
        <tr><td rowspan="1">0x29</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x2A: Entity Create {% include server-to-client.md %}

This packet creates an entity.

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
        <tr><td rowspan="1">0x2A</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x2B: Entity Update {% include server-to-client.md %}

This packet updates an entity's properties.

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
        <tr><td rowspan="1">0x2B</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x2C: Entity Destroy {% include bidirectional.md %}

This packet destroys an entity.

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
        <tr><td rowspan="1">0x2C</td><td colspan=3 align='center'>TODO</td></tr>

    </tbody>
</table>

### 0x2D: Damage Notification {% include bidirectional.md %}

This packet notifies the receiver of damage received.

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
        <tr><td rowspan="1">0x2D</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x2E: Status Effect Request {% include client-to-server.md %}

This packet requests a status effect from the server.

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
        <tr><td rowspan="1">0x2E</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x2F: Update World Properties {% include server-to-client.md %}

This packet updates world properties.

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
        <tr><td rowspan="1">0x2F</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

### 0x30: Heartbeat {% include bidirectional.md %}

This packet is periodically sent to inform the other party that the other end is still connected.

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
        <tr><td rowspan="1">0x30</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>
