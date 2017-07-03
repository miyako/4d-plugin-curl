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
