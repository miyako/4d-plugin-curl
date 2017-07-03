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
error|LONGINT|Error code

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
error|ARRAY LONGINT|Error code (out)

**Note**: The following options are not support with cURL ARRAY:

* ``CURLOPT_WRITEDATA``
* ``CURLOPT_READDATA``
* ``CURLOPT_HTTPPOST_FILE``
* ``CURLOPT_HTTPPOST``

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

### Changes

Some constants have been changed:

``CURLOPT_READDATA`` is now ``9`` instead of ``22``.

``CURLOPT_COOKIE`` remains to be ``22``.

``CURLOPT_HTTP_ACCEPT`` is now ``29`` instead of ``9``.

There was a conflict of constants since the introduction of ``CURLOPT_READDATA``.

The component can now accept a callback method that is a **shared component method**.

Added ``CURLOPT_WRITEDATA (1)`` and ``CURLOPT_READDATA (9)``. When a file path is passed, the in/out BLOBs are ignored and the file is used instead.

* Example

```
//upload
C_BLOB($in;$out)
C_LONGINT($err)
ARRAY LONGINT($optionNames;0)
ARRAY TEXT($optionValues;0)

Progress QUIT (0)

$progressId:=Progress New 
Progress SET PROGRESS ($progressId;-1)
Progress SET BUTTON ENABLED ($progressId;True)

APPEND TO ARRAY($optionNames;CURLOPT_XFERINFOFUNCTION)
APPEND TO ARRAY($optionValues;"CB_PROGRESS_DL")

APPEND TO ARRAY($optionNames;CURLOPT_READDATA)
APPEND TO ARRAY($optionValues;System folder(Desktop)+"test.jpg")
APPEND TO ARRAY($optionNames;CURLOPT_XFERINFODATA)
APPEND TO ARRAY($optionValues;String($progressId))  //will be passed as $1
APPEND TO ARRAY($optionNames;CURLOPT_POST)
APPEND TO ARRAY($optionValues;"1")

$err:=cURL ("http://localhost/test";$optionNames;$optionValues;$in;$out)

Progress QUIT ($progressId)

//download
C_BLOB($in;$out)
C_LONGINT($err)
ARRAY LONGINT($optionNames;1)
ARRAY TEXT($optionValues;1)

Progress QUIT (0)

$progressId:=Progress New 
Progress SET PROGRESS ($progressId;-1)
Progress SET BUTTON ENABLED ($progressId;True)

APPEND TO ARRAY($optionNames;CURLOPT_XFERINFOFUNCTION)
APPEND TO ARRAY($optionValues;"CB_PROGRESS_DL")
APPEND TO ARRAY($optionNames;CURLOPT_WRITEDATA)
APPEND TO ARRAY($optionValues;System folder(Desktop)+"test.jpg")
APPEND TO ARRAY($optionNames;CURLOPT_XFERINFODATA)
APPEND TO ARRAY($optionValues;String($progressId))  //will be passed as $1

$err:=cURL ("http://www.4d.com/sites/all/themes/dimention/images/home/logo4D.jpg";$optionNames;$optionValues;$in;$out)

Progress QUIT ($progressId)
```

Important
---
This plugin project is a forked subset of what was published as [OAuth](https://github.com/miyako/4d-plugin-oauth).

Existing cURL@ commands have the same name and functionality, but their tokens (internal IDs) have changed.

To migrate existing methods, do the following:

1. Comment the code that calls cURL@ plugin commands.
2. Close 4DB.
3. Replace the plugin.
4. Uncomment the code.
 
New
---

### Progress callback

```
$url:="http://www.4d.com/sites/default/files/Homepage-banner_4D-Mobile_ja.jpg"

Progress QUIT (0)

$progressId:=Progress New 
Progress SET PROGRESS ($progressId;-1)
Progress SET BUTTON ENABLED ($progressId;True)

ARRAY LONGINT($optionNames;0)
ARRAY TEXT($optionValues;0)

APPEND TO ARRAY($optionNames;CURLOPT_XFERINFOFUNCTION)
APPEND TO ARRAY($optionValues;"CB_PROGRESS_DL")

APPEND TO ARRAY($optionNames;CURLOPT_XFERINFODATA)
APPEND TO ARRAY($optionValues;String($progressId))  //will be passed as $1

$err:=cURL ($url;$optionNames;$optionValues;$in;$out)

Progress QUIT ($progressId)
```

``CB_PROGRESS_DL``

```
C_LONGINT($1;$2;$3;$4;$5)
C_BOOLEAN($0)

$progressId:=$1
  //download info
$dltotal:=$2
$dlnow:=$3
  //upload info
  //$ultotal:=$4
  //$ulnow:=$5

$progress:=Choose($dltotal#0;$dlnow/$dltotal;-1)
$message:=Choose($progress#-1;String($dlnow)+"/"+String($dltotal)+" bytes downloaded";"connecting...")

Progress SET PROGRESS ($progressId;$progress;$message)

$0:=Progress Stopped ($progressId)
```

### Header callback

```
C_BLOB($in;$out)
C_LONGINT($err)

ARRAY LONGINT($optionNames;0)
ARRAY TEXT($optionValues;0)

APPEND TO ARRAY($optionNames;CURLOPT_HEADERFUNCTION)
APPEND TO ARRAY($optionValues;"CB_HEADER")

$err:=cURL ("http://www.4d.com/sites/all/themes/dimention/images/home/logo4D.jpg";$optionNames;$optionValues;$in;$out)

```

``CB_HEADER``

```
C_BLOB($1)
C_BOOLEAN($0)

$header:=BLOB to text($1;Mac text without length)

MESSAGE($header)
```

### cURL Get executable

Returns the path to the curl executable embedded in the plugin. You can use this with LAUNCH EXTERNAL PROCESS.

### cURL Get version 

```
$version:=cURL Get version 
  //libcurl/7.40.0 OpenSSL/1.0.1j zlib/1.2.8 libidn/1.29 libssh2/1.4.3

$path:=cURL Get executable 

$stdOut:=""
$stdIn:=""
$stdErr:=""

LAUNCH EXTERNAL PROCESS($path+" -V";$stdIn;$stdOut;$stdErr);
  //curl 7.40.0 (x86_64-apple-darwin14.0.0) libcurl/7.40.0 OpenSSL/1.0.1j zlib/1.2.8 libidn/1.29 libssh2/1.4.3\nProtocols: dict file ftp ftps gopher http https imap imaps ldap ldaps pop3 pop3s rtsp scp sftp smb smbs smtp smtps telnet tftp \nFeatures: IDN IPv6 Largefile NTLM NTLM_WB SSL libz TLS-SRP UnixSockets \n
```

### Debug callback

```
C_BLOB($in;$out)
C_LONGINT($err)
ARRAY LONGINT($tNomOption;0)
ARRAY TEXT($tValOption;0)
APPEND TO ARRAY($tNomOption;CURLOPT_SSL_VERIFYHOST)
APPEND TO ARRAY($tValOption;"1")

APPEND TO ARRAY($tNomOption;CURLOPT_SSL_VERIFYPEER)
APPEND TO ARRAY($tValOption;"1")
APPEND TO ARRAY($tNomOption;CURLOPT_CAINFO)
APPEND TO ARRAY($tValOption;Convert path system to POSIX(Get 4D folder(Current resources folder)+"cacert.pem"))

APPEND TO ARRAY($tNomOption;CURLOPT_DEBUGFUNCTION)
APPEND TO ARRAY($tValOption;"CB_DEBUG")

$err:=cURL ("https://github.com/";$tNomOption;$tValOption;$in;$out)
```

``CB_DEBUG``

```
C_LONGINT($1)
C_BLOB($2)

$infoType:=Choose($1;"info";"headerIn";"headerOut";"dataIn";"dataOut")
$message:=BLOB to text($2;Mac text without length)

MESSAGE($infoType+": "+$message)
```

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

### New!

A new 6th parameter has been added to return more information (HTTP status, etc).

The following options are returned:

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
