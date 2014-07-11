---
layout: base
title: Starbound Networking
---
{% capture content %}
# Starbound Networking

The Starbound networking protocol is based on TCP, and uses port 21025 by default. The current protocol version (Enraged Koala) is 639.

## Resources

There are several related pages that you might want to browse:

* [Data types](/networking/data-types.html)
* [Logging on to a server](/networking/connecting.html)

## Base Packet

Each packet is wrapped in this basic package:

<table class="table table-bordered">
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

Though the server and client must be able to handle compressed packets, no packet *must* be compressed. Packets are compressed in the official client and server when it finds that the compressed payload is smaller than the uncompress payload.

## Packets

{% include packet-header.html name="Protocol Version" id="0" direction="server-to-client" %}

This packet is the first packet sent. It contains the server version.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="2">0</td></tr>
        <tr>
            <td>uint32</td>
            <td>Protocol version number</td>
            <td>The server's supported protocol version. Changes with each release.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Connection Response" id="1" direction="server-to-client" %}

This packet tells the client whether their connection attempt is successful or if they have been rejected. It is the final packet sent in the handshake process.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="18">1</td></tr>
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
        <tr>
            <td>bool</td>
            <td>Celestial Info Exists</td>
            <td>Determines whether the following celestial information exists.</td>
        </tr>
        <tr>
            <td>int32</td>
            <td>Orbital Levels</td>
            <td>The maximum number of orbital levels.</td>
        </tr>
        <tr>
            <td>int32</td>
            <td>Chunk Size?</td>
            <td></td>
        </tr>
        <tr>
            <td>int32</td>
            <td>XY Coordinate Min</td>
            <td></td>
        </tr>
        <tr>
            <td>int32</td>
            <td>XY Coordinate Max</td>
            <td></td>
        </tr>
        <tr>
            <td>int32</td>
            <td>Z Coordinate Min</td>
            <td></td>
        </tr>
        <tr>
            <td>int32</td>
            <td>Z Coordinate Max</td>
            <td></td>
        </tr>
        <tr>
            <td>VLQ</td>
            <td>Number of Sectors</td>
            <td>The number of sectors in the following list.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Sector Id</td>
            <td>The sector id used to identify coordinates. Example: alpha</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Sector Name</td>
            <td>The sector name as it appears in the navigation pane? Example: Alpha Sector</td>
        </tr>
        <tr>
            <td>uint64</td>
            <td>Sector Seed</td>
            <td>The sector seed used to generate the world.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Sector Prefix</td>
            <td>The sector prefix as it appears in names. Example: Alpha</td>
        </tr>
        <tr>
            <td>Variant</td>
            <td>Parameters</td>
            <td></td>
        </tr>
        <tr>
            <td>Variant</td>
            <td>Sector Config</td>
            <td>The sector config variant from celestial.config.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Disconnect Response" id="2" direction="server-to-client" %}

This packet is used to notify the client of a disconnect.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="2">2</td></tr>
        <tr>
            <td>uint8</td>
            <td>Unknown</td>
            <td></td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Handshake Challenge" id="3" direction="server-to-client" %}

This packet provides a salt and round count for password verification. It is followed by [Handshake Response](#handshake-response).

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">3</td></tr>
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

{% include packet-header.html name="Chat Recieved" id="4" direction="server-to-client" %}

This packet is sent to a client with a chat message.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="6">4</td></tr>
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
            <td>Message</td>
            <td>The chat message.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Universe Time Update" id="5" direction="server-to-client" %}

This packet is sent from the server to update the current time.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="2">5</td></tr>
        <tr>
            <td>sVLQ</td>
            <td>Time</td>
            <td></td>
        </tr>
    </tbody>
</table>


{% include packet-header.html name="Celestial Response" id="6" direction="server-to-client" %}

This packet has yet to be fully understood.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">6</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Client Connect" id="7" direction="client-to-server" %}

This packet is sent in the handshake process immediately after the [Protocol Version](#protocol-version). It contains all relevant data about the connecting player.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="9">7</td></tr>
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
            <td>The player's account name, from the connection dialog.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Client Disconnect" id="8" direction="client-to-server" %}

This packet is sent when the client disconnects.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="2">8</td></tr>
        <tr>
            <td>uint8</td>
            <td>Unknown</td>
            <td></td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Handshake Response" id="9" direction="client-to-server" %}

This packet is the response to the [Handshake Challenge](#handshake-challenge).

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="3">9</td></tr>
        <tr>
            <td>string</td>
            <td>Claim Response</td>
            <td>A response to the claim message. Currently unused.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Password hash</td>
            <td>The hash of the entered password, salted with the information in <a href="#handshake-challenge">Handshake Challenge</a>.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Warp Command" id="10" direction="bidirectional" %}

This packet is sent when the player warps/is warped to a planet or ship.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">10</td></tr>
        <tr>
            <td>uint32</td>
            <td>Warp type</td>
            <td>The type of warp being done.</td>
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

#### Warp Types

<table class="table">
    <thead>
        <tr>
            <th>Value</th>
            <th>Name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td>
            <td>Move ship</td>
        </tr>
        <tr>
            <td>2</td>
            <td>Warp to own ship</td>
        </tr>
        <tr>
            <td>3</td>
            <td>Warp to player ship</td>
        </tr>
        <tr>
            <td>4</td>
            <td>Warp to orbited planet</td>
        </tr>
        <tr>
            <td>5</td>
            <td>Warp to home planet</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Chat Sent" id="11" direction="client-to-server" %}

This packet is sent from the client whenever a message is sent in the chat window.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="3">11</td></tr>
        <tr>
            <td>string</td>
            <td>Message</td>
            <td>The chat message being sent.</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Channel</td>
            <td>The current chat channel.</td>
        </tr>
    </tbody>
</table>


{% include packet-header.html name="Celestial Request" id="12" direction="client-to-server" %}

This packet has yet to be fully understood.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">12</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Client Context Update" id="13" direction="bidirectional" %}

This packet has yet to be fully understood.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="2">13</td></tr>
        <tr>
            <td>uint8[]</td>
            <td>Client Context Data</td>
            <td>Myriad formats.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="World Start" id="14" direction="server-to-client" %}

This packet is sent to the client when a world thread has been started on the server.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="10">14</td></tr>
        <tr>
            <td>Variant</td>
            <td>Planet</td>
            <td>The world data.</td>
        </tr>
        <tr>
            <td>Variant</td>
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
        <tr>
            <td>Variant</td>
            <td>World Properties</td>
            <td>A dictionary with multiple key value pairs about world properties. See <a href="#0x2F">Update World Properties</a>.</td>
        </tr>
        <tr>
            <td>uint32</td>
            <td>Client ID</td>
            <td>The client's ID.</td>
        </tr>
        <tr>
            <td>bool</td>
            <td>Local</td>
            <td>Determines whether the interpolation settings used are for a local connection.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="World Stop" id="15" direction="server-to-client" %}

Called when a world thread is stopped.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="2">15</td></tr>
        <tr>
            <td>string</td>
            <td>Status</td>
            <td>The status of the world.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Tile Array Update" id="16" direction="server-to-client"%}

Called when an array of tiles has its properties updated.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="6">16</td></tr>
        <tr>
            <td>sVLQ</td>
            <td>Tile X</td>
            <td>The X coordinate within the server's tile array.</td>
        </tr>
        <tr>
            <td>sVLQ</td>
            <td>Tile Y</td>
            <td>The Y coordinate within the server's tile array.</td>
        </tr>
        <tr>
            <td>VLQ</td>
            <td>Width</td>
            <td></td>
        </tr>
        <tr>
            <td>VLQ</td>
            <td>Height</td>
            <td></td>
        </tr>
        <tr>
            <td><a href="/networking/data-types.html">NetTile</a>[Width,Height]</td>
            <td>Tiles</td>
            <td>The array of updated tiles.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Tile Update" id="17" direction="server-to-client" %}

This packet is called when a tile is updated.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">17</td></tr>
        <tr>
            <td>sVLQ</td>
            <td>Tile X</td>
            <td>The tile's X coordinate within the server's tile array.</td>
        </tr>
        <tr>
            <td>sVLQ</td>
            <td>Tile Y</td>
            <td>The tile's Y coordinate within the server's tile array.</td>
        </tr>
        <tr>
            <td><a href="/networking/data-types.html">NetTile</a></td>
            <td>Tile</td>
            <td>The updated tile.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Tile Liquid Update" id="18" direction="server-to-client" %}

This packet is sent when the liquid on a tile has changed position.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="5">18</td></tr>
        <tr>
            <td>sVLQ</td>
            <td>Tile X</td>
            <td>The tile's X coordinate within the server's tile array.</td>
        </tr>
        <tr>
            <td>sVLQ</td>
            <td>Tile Y</td>
            <td>The tile's Y coordinate within the server's tile array.</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Unknown</td>
            <td>Possibly the liquid level.</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Unknown</td>
            <td>Possibly the liquid type.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Tile Damage Update" id="19" direction="server-to-client" %}

This packet is sent when a tile is damaged.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="9">19</td></tr>
        <tr>
            <td>int32</td>
            <td>Tile X</td>
            <td>The tile's X coordinate within the server's tile array.</td>
        </tr>
        <tr>
            <td>int32</td>
            <td>Tile Y</td>
            <td>The tile's Y coordinate within the server's tile array.</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Unknown</td>
            <td></td>
        </tr>
        <tr>
            <td>float</td>
            <td>Damage Percentage</td>
            <td></td>
        </tr>
        <tr>
            <td>float</td>
            <td>Damage Effect Percentage</td>
            <td></td>
        </tr>
        <tr>
            <td>float</td>
            <td>Source Position X</td>
            <td></td>
        </tr>
        <tr>
            <td>float</td>
            <td>Source Position Y</td>
            <td></td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Damage Type</td>
            <td></td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Tile Modification Failure" id="20" direction="server-to-client" %}

This packet is sent when a tile list cannot successfully be modified.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">20</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Give Item" id="21" direction="server-to-client" %}

This packet attempts to give an item to a player. If the player's inventory is full, it will drop on the ground next to them.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">21</td></tr>
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

{% include packet-header.html name="Swap in Container Result" id="22" direction="server-to-client" %}

This packet is sent whenever two items are swapped in an open container.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">22</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Environment Update" id="23" direction="server-to-client" direction="server-to-client" %}

This packet is sent on an environment update.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">23</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Entity Interact Result" id="24" direction="server-to-client" %}

This packet contains the results of an entity interaction.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">24</td></tr>
        <tr>
            <td>uint32</td>
            <td>Client ID</td>
            <td>The client attempting to interact.</td>
        </tr>
        <tr>
            <td>int32</td>
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

{% include packet-header.html name="Modify Tile List" id="25" direction="client-to-server" %}

This packet contains a list of tiles and modifications to them.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">25</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Damage Tile" id="26" direction="client-to-server" %}

This packet updates a tile's damage

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">26</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Damage Tile Group" id="27" direction="client-to-server" %}

This packet updates an entire tile group's damage.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">27</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Request Drop" id="28" direction="client-to-server" %}

This packet requests an item drop from the ground.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="2">28</td></tr>
        <tr>
            <td>sVLQ</td>
            <td>Entity ID</td>
            <td>The entity ID of the item drop.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Spawn Entity" id="29" direction="client-to-server" %}

This packet requests that the server spawn an entity.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">29</td><td colspan=3 align='center'>TODO</td></tr>
</table>

{% include packet-header.html name="Entity Interact" id="30" direction="client-to-server" %}

This packet is sent when a client attempts to interact with an entity.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">30</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Connect Wire" id="31" %}

This packet connects a wire.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">31</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Disconnect All Wires" id="32" %}

This packet disconnects all wires.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">32</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Open Container" id="33" direction="client-to-server" %}

This packet opens a container.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="2">33</td></tr>
        <tr>
            <td>sVLQ</td>
            <td>Entity ID</td>
            <td>The entity ID of the container.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Close Container" id="34" direction="client-to-server" %}

This packet closes a container.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="2">34</td></tr>
        <tr>
            <td>sVLQ</td>
            <td>Entity ID</td>
            <td>The entity ID of the container.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Swap in Container" id="35" direction="client-to-server" %}

This packet swaps an item in a container.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">35</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Item Apply in Container" id="36" direction="client-to-server" %}

This packet applies an item to another item in a container.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="1">36</td><td colspan=3 align='center'>TODO</td></tr>
    </tbody>
</table>

{% include packet-header.html name="Start Crafting in Container" id="37" direction="client-to-server" %}

This packet initiates crafting on an item in a container (Used in pixel compressors and the like?)

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="2">37</td></tr>
        <tr>
            <td>sVLQ</td>
            <td>Entity ID</td>
            <td>The entity ID of the container.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Stop Crafting in Container" id="38" direction="client-to-server" %}

This packet stops crafting on an item in a container

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="2">38</td></tr>
        <tr>
            <td>sVLQ</td>
            <td>Entity ID</td>
            <td>The entity ID of the container.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Burn Container" id="39" direction="client-to-server" %}

This packet burns a container.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="2">39</td></tr>
        <tr>
            <td>sVLQ</td>
            <td>Entity ID</td>
            <td>The entity ID of the container.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Clear Container" id="40" direction="client-to-server" %}

This packet clears a container.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="2">40</td></tr>
        <tr>
            <td>sVLQ</td>
            <td>Entity ID</td>
            <td>The entity ID of the container.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="World Client State Update" id="41" direction="client-to-server" %}

This packet contains a world client state update

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="2">41</td></tr>
        <tr>
            <td>uint8[]</td>
            <td>Delta</td>
            <td>The world's state delta.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Entity Create" id="42" direction="bidirectional" %}

This packet creates an entity.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">42</td></tr>
        <tr>
            <td>uint8</td>
            <td>Entity Type</td>
            <td>The type of entity.</td>
        </tr>
        <tr>
            <td>uint8[]</td>
            <td>Store Data</td>
            <td>The entity's store data.</td>
        </tr>
        <tr>
            <td>sVLQ</td>
            <td>Entity ID</td>
            <td>The entity ID.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Entity Update" id="43" direction="bidirectional" %}

This packet updates an entity's properties.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="3">43</td></tr>
        <tr>
            <td>sVLQ</td>
            <td>Entity ID</td>
            <td>The ID of the entity.</td>
        </tr>
        <tr>
            <td>uint8[]</td>
            <td>Delta</td>
            <td>The entity's state delta.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Entity Destroy" id="44" direction="server-to-client" %}

This packet destroys an entity.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="3">44</td></tr>
        <tr>
            <td>sVLQ</td>
            <td>Entity ID</td>
            <td>The ID of the entity being destroyed.</td>
        </tr>
        <tr>
            <td>bool</td>
            <td>Death</td>
            <td>Determines whether this entity was destroyed via death.</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Damage Notification" id="45" direction="bidirectional" %}

This packet notifies the receiver of damage received.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="10">45</td></tr>
        <tr>
            <td>sVLQ</td>
            <td>Causing Entity ID</td>
            <td>The ID of the entity causing the damage.</td>
        </tr>
        <tr>
            <td>sVLQ</td>
            <td>Target Entity ID</td>
            <td>The ID of the entity being targeted.</td>
        </tr>
        <tr>
            <td>sVLQ</td>
            <td>Position X*</td>
            <td>The X position of the damage.</td>
        </tr>
        <tr>
            <td>sVLQ</td>
            <td>Position Y*</td>
            <td>The Y position of the damage.</td>
        </tr>
        <tr>
            <td>sVLQ</td>
            <td>Damage*</td>
            <td>The amount of damage.</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Damage Kind</td>
            <td>The kind of damage.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Damage Source Kind</td>
            <td>The kind of damage source.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Target Material Kind</td>
            <td>The kind of material the target is made of.</td>
        </tr>
        <tr>
            <td>uint8</td>
            <td>Hit Result Kind</td>
            <td>The kind of damage hit result.</td>
        </tr>
    </tbody>
</table>

*: These values are divided by 100 when read, and multiplied by 100 written.

#### Damage Kinds

<table class="table">
    <thead>
        <tr>
            <th>Value</th>
            <th>Name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>Normal</td>
        </tr>
        <tr>
            <td>1</td>
            <td>Shield</td>
        </tr>
        <tr>
            <td>2</td>
            <td>Energy</td>
        </tr>
    </tbody>
</table>

#### Hit Result Kinds

<table class="table">
    <thead>
        <tr>
            <th>Value</th>
            <th>Name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>None</td>
        </tr>
        <tr>
            <td>1</td>
            <td>Hit</td>
        </tr>
        <tr>
            <td>2</td>
            <td>Kill</td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Status Effect Request" id="46" direction="client-to-server" %}

This packet requests a status effect from the server.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="5">46</td></tr>
        <tr>
            <td>sVLQ</td>
            <td>Unknown</td>
            <td>Possibly the entity ID</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Status Effect Name</td>
            <td>The name of the status effect.</td>
        </tr>
        <tr>
            <td>Variant</td>
            <td>Unknown</td>
            <td>Possibly the parameters to the status effect.</td>
        </tr>
        <tr>
            <td>float</td>
            <td>Multiplier</td>
            <td></td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Update World Properties" id="47" direction="server-to-client" %}

This packet updates world properties.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="4">47</td></tr>
        <tr>
            <td>VLQ</td>
            <td>Number of pairs</td>
            <td>The number of names/value pairs in this packet.</td>
        </tr>
        <tr>
            <td>string</td>
            <td>Property name</td>
            <td>The name of the property that is about to be set. Example: fuel.level</td>
        </tr>
        <tr>
            <td>Variant</td>
            <td>Property value</td>
            <td>The new value of the before mentioned property. Example: 800 </td>
        </tr>
    </tbody>
</table>

{% include packet-header.html name="Heartbeat" id="48" direction="bidirectional" %}

This packet is periodically sent to inform the other party that the other end is still connected.

<table class="table table-bordered packet">
    <thead>
        <tr>
            <th>Packet ID</th>
            <th>Type</th>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr><td rowspan="2">48</td></tr>
        <tr>
            <td>VLQ</td>
            <td>Current Step</td>
            <td>The current heartbeat step.</td>
        </tr>
    </tbody>
</table>
{% endcapture %}

<!--
    This stuff just defines the layout for the rest. We have to put it down here because
    jekyll can be a bit of a pain sometimes.
-->
<div class="row">
    <div class="col-md-3">
        <ul class="side-nav nav">
            <h4>Packets</h4>
            {{ packets }}
        </ul>
    </div>
    <div class="col-md-9">
        {{ content | markdownify }}
    </div>
</div>
