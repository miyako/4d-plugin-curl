# 4d-plugin-curl
4D implementation of [curl](http://curl.haxx.se)

### Platform

| carbon | cocoa | win32 | win64 |
|:------:|:-----:|:---------:|:---------:|
|<img src="https://cloud.githubusercontent.com/assets/1725068/22371562/1b091f0a-e4db-11e6-8458-8653954a7cce.png" width="24" height="24" />|<img src="https://cloud.githubusercontent.com/assets/1725068/22371562/1b091f0a-e4db-11e6-8458-8653954a7cce.png" width="24" height="24" />|<img src="https://cloud.githubusercontent.com/assets/1725068/22371562/1b091f0a-e4db-11e6-8458-8653954a7cce.png" width="24" height="24" />|<img src="https://cloud.githubusercontent.com/assets/1725068/22371562/1b091f0a-e4db-11e6-8458-8653954a7cce.png" width="24" height="24" />|

### Version

<img src="https://cloud.githubusercontent.com/assets/1725068/18940649/21945000-8645-11e6-86ed-4a0f800e5a73.png" width="32" height="32" /> <img src="https://cloud.githubusercontent.com/assets/1725068/18940648/2192ddba-8645-11e6-864d-6d5692d55717.png" width="32" height="32" />

## Syntax

```
error:=cURL (url;names;values;in;out;info)
```

Parameter|Type|Description
------------|------------|----
url|TEXT|Request URL (in)
names|ARRAY LONGINT|Option names (in)
values|ARRAY TEXT|Option values (in)
in|BLOB|Request data (in)
out|BLOB|Response data (out)
info|TEXT|Information in JSON (out)
error|LONGINT|[Error code](https://curl.haxx.se/libcurl/c/libcurl-errors.html)

```
cURL ARRAY (url;names;values;in;out;info;error)
```

Parameter|Type|Description
------------|------------|----
url|ARRAY TEXT|Request URL (in)
names|ARRAY LONGINT|Option names (in)
values|ARRAY TEXT|Option values (in)
in|ARRAY BLOB|Request data (in)
out|ARRAY BLOB|Response data (out)
info|ARRAY TEXT|Information in JSON (out)
error|ARRAY LONGINT|[Error code](https://curl.haxx.se/libcurl/c/libcurl-errors.html)

**Note**: The following options are not support with cURL ARRAY:

* ``CURLOPT_WRITEDATA``
* ``CURLOPT_READDATA``
* ``CURLOPT_HTTPPOST_FILE``
* ``CURLOPT_HTTPPOST``

The following options are returned in ``info``:

```
conditionUnmet
contentLengthUpload
rtspClientCseq
rtspServerCseq
rtspCseqRecv
lastSocket
primaryPort
localPort
contentLengthDownload
connectCode
fileTime
totalTime
requestSize
headerSize
speedUpload
speedDownload
sizeDownload
sizeUpload
httpAuthAvail
proxyAuthAvail
osErrNo
numConnects
responseCode
nameLookupTime
connectTime
appConnectTime
preTransferTime
startTransferTime
redirectTime
sslVerifyResult
redirectCount
effectiveUrl
localIp
contentType
primaryIp
redirectUrl
ftpEntryPath
rtspSessionId
```

```
escaped:=cURL Escape url (url)
unescaped:=cURL Unescape url (url)
```

Parameter|Type|Description
------------|------------|----
url|TEXT|URL (in)
escaped|TEXT|Processed URL
unescaped|TEXT|Processed URL

```
version:=cURL Get version
```

Parameter|Type|Description
------------|------------|----
version|TEXT|Library version

```
success:=cURL Get date (date;time)
```

Parameter|Type|Description
------------|------------|----
date|TEXT|[Date string](https://curl.haxx.se/libcurl/c/curl_getdate.html) (in)
time|LONGINT|Number of seconds since the Epoch, January 1st 1970 00:00:00 in the UTC time zone (out)
success|LONGINT|``0`` on error, ``1`` on success

```
path:=cURL Get executable
```

Parameter|Type|Description
------------|------------|----
path|TEXT|Path to command line program inside the plugin

Returns the path to the curl executable embedded in the plugin. You can use this with LAUNCH EXTERNAL PROCESS.

```
version:=cURL Get version
```

Parameter|Type|Description
------------|------------|----
version|TEXT|

|Protocol|Mac OS X|Windows|
|:-------:|:-:|:-----:|
|dict|◯|◯|
|file|◯|◯|
|ftp|◯|◯|
|ftps|◯|◯|
|gopher|◯|◯|
|http|◯|◯|
|https|◯|◯|
|imap|◯|◯|
|imaps|◯|◯|
|ldap|◯|◯|
|ldaps|◯|◯|
|pop3|◯|◯|
|pop3s|◯|◯|
|rtsp|◯|◯|
|scp|◯|◯|
|sftp|◯|◯|
|smtp|◯|◯|
|smtps|◯|◯|
|telnet|◯|◯|
|tftp|◯|◯|

|Feature|Mac OS X|Windows|
|:-----:|:-:|:-----:|
|IDN|◯|◯|
|IPv6|◯|◯|
|Largefile|◯|◯|
|NTLM|◯|◯|
|SSL|◯|◯|
|libz|◯|◯|

Remarks
---
On Mac, SSH2 is linked to the system OpenSSL located at /usr/lib/, not the one embedded in the plugin. [This is to avoid crash with SFTP](http://forums.4d.fr/Post/FR/15200699/1/15251183).

On Windows, OPENSSL (LIBEAY and LIBSSL) and LIBSSH2 are statically linked to LIBCURL, to avoid collision with the DLL included in 4D itself.

LIBCURL is modified so that it will yield to 4D during a long operation (easy.c).
