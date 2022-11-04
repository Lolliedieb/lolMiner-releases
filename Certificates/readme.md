# lolMiner certificate file

The file **lolMiner.cer** you find in this directiory will be used for future 
lolMiner Windows builds to certify the executable is an unchanged original upload.

Use
```
certutil -user -addstore Root lolMiner.cer
```
to install the file on your Windows system to check the lolMiner.exe is an original.

This will work with lolMiner 1.61 (reuploaded version) and later.
