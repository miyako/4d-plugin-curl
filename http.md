## HTTP

It would be wise to implement a reasonable timeout in case the server is down:

```
C_BLOB($in;$out)
C_LONGINT($err)
ARRAY LONGINT($optionNames;0)
ARRAY TEXT($optionValues;0)

APPEND TO ARRAY($optionNames;CURLOPT_CONNECTTIMEOUT)
APPEND TO ARRAY($optionValues;"3")  //default is 300 seconds

APPEND TO ARRAY($optionNames;CURLOPT_TIMEOUT)
APPEND TO ARRAY($optionValues;"30")  //default is unlimited

APPEND TO ARRAY($optionNames;CURLOPT_LOW_SPEED_TIME)
APPEND TO ARRAY($optionValues;"3")  //default is unlimited

APPEND TO ARRAY($optionNames;CURLOPT_LOW_SPEED_LIMIT)
APPEND TO ARRAY($optionValues;"4096")  //default is unlimited

$err:=cURL ("http://www.4d.com/sites/all/themes/dimention/images/home/logo4D.jpg";$optionNames;$optionValues;$in;$out)

If ($err=0)
	
	$path:=System folder(Desktop)+"logo4D.jpg"
	
	BLOB TO DOCUMENT($path;$out)
	
	SHOW ON DISK($path)
	
End if 
```

It is more efficient to perform multiple gets in one call:

```
C_BLOB($in;$out)
C_LONGINT($err)
ARRAY LONGINT($optionNames;0)
ARRAY TEXT($optionValues;0)

APPEND TO ARRAY($optionNames;CURLOPT_SSL_VERIFYHOST)
APPEND TO ARRAY($optionValues;"1")
APPEND TO ARRAY($optionNames;CURLOPT_SSL_VERIFYPEER)
APPEND TO ARRAY($optionValues;"1")
APPEND TO ARRAY($optionNames;CURLOPT_CAINFO)
APPEND TO ARRAY($optionValues;Convert path system to POSIX(Get 4D folder(Current resources folder)+"cacert.pem"))

APPEND TO ARRAY($optionNames;CURLOPT_TCP_KEEPALIVE)
APPEND TO ARRAY($optionValues;"1")
APPEND TO ARRAY($optionNames;CURLOPT_TCP_KEEPIDLE)
APPEND TO ARRAY($optionValues;"120")
APPEND TO ARRAY($optionNames;CURLOPT_TCP_KEEPINTVL)
APPEND TO ARRAY($optionValues;"60")

APPEND TO ARRAY($optionNames;CURLOPT_ACCEPT_ENCODING)
APPEND TO ARRAY($optionValues;"gzip,deflate")

APPEND TO ARRAY($optionNames;CURLOPT_CONNECTTIMEOUT)
APPEND TO ARRAY($optionValues;"3")  //default is 300 seconds

APPEND TO ARRAY($optionNames;CURLOPT_TIMEOUT)
APPEND TO ARRAY($optionValues;"30")  //default is unlimited

APPEND TO ARRAY($optionNames;CURLOPT_LOW_SPEED_TIME)
APPEND TO ARRAY($optionValues;"3")  //default is unlimited

APPEND TO ARRAY($optionNames;CURLOPT_LOW_SPEED_LIMIT)
APPEND TO ARRAY($optionValues;"4096")  //default is unlimited

ARRAY TEXT($URLs;3)

$URLs{0}:="https://twitter.com"
$URLs{1}:="https://twitter.com"
$URLs{2}:="https://twitter.com"
$URLs{3}:="https://twitter.com"

ARRAY BLOB($ins;0)
ARRAY BLOB($outs;0)
ARRAY TEXT($jsons;0)
ARRAY LONGINT($errs;0)

$start:=Milliseconds
cURL ARRAY ($URLs;$optionNames;$optionValues;$ins;$outs;$jsons;$errs)
$duration:=Milliseconds-$start
```
