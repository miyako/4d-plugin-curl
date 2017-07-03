Important
---
This plugin project is a forked subset of what was published as [OAuth](https://github.com/miyako/4d-plugin-oauth).

Existing cURL@ commands have the same name and functionality, but their tokens (internal IDs) have changed.

To migrate existing methods, do the following:

1. Comment the code that calls cURL@ plugin commands.
2. Close 4DB.
3. Replace the plugin.
4. Uncomment the code.

### Changes

Some constants have been changed:

``CURLOPT_READDATA`` is now ``9`` instead of ``22``.

``CURLOPT_COOKIE`` remains to be ``22``.

``CURLOPT_HTTP_ACCEPT`` is now ``29`` instead of ``9``.

There was a conflict of constants since the introduction of ``CURLOPT_READDATA``.

The component can now accept a callback method that is a **shared component method**.

Added ``CURLOPT_WRITEDATA (1)`` and ``CURLOPT_READDATA (9)``. When a file path is passed, the in/out BLOBs are ignored and the file is used instead.
