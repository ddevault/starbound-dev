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

{% include packet-header.html name="Protocol Version" id="01" direction="server-to-client" %}

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

{% include packet-header.html name="Connection Response" id="02" direction="server-to-client" %}

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

{% include packet-header.html name="Server Disconnect" id="03" direction="server-to-client" %}

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

{% include packet-header.html name="Handshake Challenge" id="04" direction="server-to-client" %}

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

{% include packet-header.html name="Chat Recieved" id="05" direction="server-to-client" %}

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

{% include packet-header.html name="Universe Time Update" id="06" direction="server-to-client" %}

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

{% include packet-header.html name="Client Connect" id="07" direction="client-to-server" %}

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

{% include packet-header.html name="Client Disconnect" id="08" direction="client-to-server" %}

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

{% include packet-header.html name="Handshake Response" id="09" direction="client-to-server" %}

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

{% include packet-header.html name="Warp Command" id="0A" direction="bidirectional" %}

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

{% include packet-header.html name="Chat Sent" id="0B" direction="client-to-server" %}

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

{% include packet-header.html name="Client Context Update" id="0C" %}

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

{% include packet-header.html name="World Start" id="0D" direction="server-to-client" %}

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

{% include packet-header.html name="World Stop" id="0E" direction="server-to-client" %}

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

{% include packet-header.html name="Tile Array Update" id="0F" %}

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

{% include packet-header.html name="Tile Update" id="10" %}

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

{% include packet-header.html name="Tile Liquid Update" id="11" %}

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

{% include packet-header.html name="Tile Damage Update" id="12" %}

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

{% include packet-header.html name="Tile Modification Failure" id="13" direction="server-to-client" %}

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

{% include packet-header.html name="Give Item" id="14" direction="server-to-client" %}

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

{% include packet-header.html name="Unknown" id="15" %}

The purpose of this packet is totally unknown. It shows up infrequently/never.

{% include packet-header.html name="Swap in Container Result" id="16" %}

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

{% include packet-header.html name="Enviornment Update" id="17" direction="server-to-client" %}

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

{% include packet-header.html name="Entity Interact Result" id="18" direction="server-to-client" %}

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

{% include packet-header.html name="Modify Tile List" id="19" direction="client-to-server" %}

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

{% include packet-header.html name="Damage Tile" id="1A" direction="client-to-server" %}

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

{% include packet-header.html name="Damage Tile Group" id="1B" direction="client-to-server" %}

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

{% include packet-header.html name="Request Drop" id="1C" direction="server-to-client" %}

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

{% include packet-header.html name="Spawn Entity" id="1D" direction="client-to-server" %}

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

{% include packet-header.html name="Entity Interact" id="1E" direction="client-to-server" %}

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

{% include packet-header.html name="Connect Wire" id="1F" %}

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

{% include packet-header.html name="Disconnect All Wires" id="20" %}

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

{% include packet-header.html name="Open Container" id="21" direction="client-to-server" %}

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

{% include packet-header.html name="Close Container" id="22" direction="client-to-server" %}

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

{% include packet-header.html name="Swap in Container" id="23" direction="client-to-server" %}

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

{% include packet-header.html name="Item Apply in Container" id="24" direction="client-to-server" %}

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

{% include packet-header.html name="Start Crafting in Container" id="25" direction="client-to-server" %}

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

{% include packet-header.html name="Stop Crafting in Container" id="26" direction="client-to-server" %}

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

{% include packet-header.html name="Burn Container" id="27" direction="client-to-server" %}

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

{% include packet-header.html name="Clear Container" id="28" direction="client-to-server" %}

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

{% include packet-header.html name="World Update" id="29" direction="server-to-client" %}

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

{% include packet-header.html name="Entity Create" id="2A" direction="server-to-client" %}

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

{% include packet-header.html name="Entity Update" id="2B" direction="server-to-client" %}

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

{% include packet-header.html name="Entity Destroy" id="2C" direction="bidirectional" %}

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

{% include packet-header.html name="Damage Notification" id="2D" direction="bidirectional" %}

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

{% include packet-header.html name="Status Effect Request" id="2E" direction="client-to-server" %}

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

{% include packet-header.html name="Update World Properties" id="2F" direction="server-to-client" %}

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

{% include packet-header.html name="Heartbeat" id="30" direction="bidirectional" %}

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
