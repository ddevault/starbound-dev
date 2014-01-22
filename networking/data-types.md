---
title: Data Types
layout: base
---

# Data Types

Starbound uses several different data types to move information over the wire. These types are used on the [protocol](/networking) documentation.

* **string**: A length prefixed UTF-8 string, with a VLQ specifying the length.
* **(u)int##**: An integer, in big-endian order. "u" indicates unsiged and ## is the length, in bits. Examples: uint8, int32, etc.
* **(s)VLQ**: [Variable length quantity](https://en.wikipedia.org/wiki/Variable-length_quantity). "s" indicates signed.
* **(u)int##[]**: An integer array. Arrays without a fixed length are prefixed with a VLQ indicating their length. Examples: uint8[], int32[10]
* **bool**: A boolean value (true or false). Encoded as a uint8, 0 indicates false and 1 indicates true.
* **float**: 32-bit IEEE 754 floating point number.
* **double**: 64-bit IEEE 754 floating point number.
* **Variant**: Data serialization format. See below.

## Variant

TODO
