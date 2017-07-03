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
