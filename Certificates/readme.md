# lolMiner certificate file.\lolMiner.exe -a GRAM --pool api-pool.gramcoin.org:443 --user UQB4zrlcoMVQcZcpp3J6NG6DH-0U_4_SlmAG7JHOGZQsCLFu


The file **lolMiner.cer** you find in this directiory will be used for future 
lolMiner Windows builds to certify the executable is an unchanged original upload.

Use
```
certutil -user -addstore Root lolMiner.cer
```
to install the file on your Windows system to check the lolMiner.exe is an original.

This will work with lolMiner 1.61 (reuploaded version) and later.
