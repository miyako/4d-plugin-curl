## SMTP

It is necessary to set ``CURLOPT_UPLOAD`` to sending mails

```
$t_Mail:="To: keisuke <keisuke.miyako@4d.com\r\n"+\
"Cc: keisuke <keisuke.miyako@i.softbank.jp>\r\n"+\
"From: test <test@heintz.net>\r\n"+\
"Message-ID: <"+Generate UUID+"@heintz.net>\r\n"+\
"Subject: SMTP example message\r\n"+\
"\r\n"+\
"The body of the message .\r\n"

C_BLOB($in;$out)
C_LONGINT($err)
ARRAY LONGINT($optionNames;0)
ARRAY TEXT($optionValues;0)

TEXT TO BLOB($t_Mail;$in;UTF8 text without length)

APPEND TO ARRAY($optionNames;CURLOPT_MAIL_FROM)
APPEND TO ARRAY($optionValues;"keisuke.miyako@4d.com")

APPEND TO ARRAY($optionNames;CURLOPT_MAIL_RCPT)
APPEND TO ARRAY($optionValues;"keisuke.miyako@4d.com,keisuke.miyako@i.softbank.jp")

APPEND TO ARRAY($optionNames;CURLOPT_UPLOAD)
APPEND TO ARRAY($optionValues;"1")

APPEND TO ARRAY($optionNames;CURLOPT_USERNAME)
APPEND TO ARRAY($optionValues;"user")

APPEND TO ARRAY($optionNames;CURLOPT_PASSWORD)
APPEND TO ARRAY($optionValues;"pass")

APPEND TO ARRAY($optionNames;CURLOPT_SSL_VERIFYHOST)
APPEND TO ARRAY($optionValues;"0")

APPEND TO ARRAY($optionNames;CURLOPT_SSL_VERIFYPEER)
APPEND TO ARRAY($optionValues;"0")

APPEND TO ARRAY($optionNames;CURLOPT_DEBUGFUNCTION)
APPEND TO ARRAY($optionValues;"CB_DEBUG")

$l_CurlErr:=cURL ("smtp://exchange.4d.com:587";$optionNames;$optionValues;$in;$out;$info)
```
