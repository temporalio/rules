# TMPRL1106 - External storage reference could not be decoded

## Cause

The SDK found a payload marked as an external storage reference, ran the configured payload codec over it, and what
came back was not a reference. The claim identifying the stored data could not be read, so the payload could not be
retrieved.

## Description

When a payload is offloaded to external storage it is replaced by a small reference holding the driver name and the
claim data. If a payload codec is configured, the SDK encodes that reference as well, so claim data does not sit in
history in the clear. The encoded reference carries a metadata marker, which lets a reader recognize it without
decoding anything first.

A reader that sees the marker therefore has to decode the payload before it can read the reference. This rule is
reported when that decode produces something that is not a reference. The usual causes are a configuration mismatch
between the worker or client that wrote the payload and the one reading it:

* No payload codec is configured on the reader, but the writer had one.
* A different codec is configured, or the same codec with different key material, so it cannot recover what the writer
  produced.
* The codec is configured but failing, for example because a remote key or decryption service is unavailable.

## Remediation

Configure the reader with the same payload codec as the writer, including the same key material, and confirm any
service the codec depends on is reachable.

If the reader is intended to run without a codec, then the writer must not have one either. Both ends of a workflow or
activity have to agree, the same way they do for payloads that were never offloaded.
