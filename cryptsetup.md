解密磁盘
```bash
cryptsetup luksOpen /dev/deviceName newName
# Enter passphrase for /dev/deviceName: password
# luksOpen 这里要区分大小写
# newName 会在/dev/mapper
# mount /dev/mapper/newName mountDirectory 挂载硬盘
```

