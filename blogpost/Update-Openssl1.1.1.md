# Installing OpenSSL from source
Download from source
```
wget https://www.openssl.org/source/openssl-1.1.1a.tar.gz
```

Check the cheksum vs the download file
```
$ curl https://www.openssl.org/source/openssl-1.1.1a.tar.gz.sha256
fc20130f8b7cbd2fb918b2f14e2f429e109c31ddd0fb38fc5d71d9ffed3f9f41


$ sha256sum openssl-1.1.1a.tar.gz
fc20130f8b7cbd2fb918b2f14e2f429e109c31ddd0fb38fc5d71d9ffed3f9f41  openssl-1.1.1a.tar.gz

```
Unpack the tar
```
tar -xzvf openssl-1.1.1a.tar.gz
```