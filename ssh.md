```
# /etc/ssh/sshd_config

PermitRootLogin yes # 允许root用户登录
```

```bash
ssh-keygen
# 生成公钥和密钥对
# id_rsa 是密钥
# id_rsa.pub 是公钥
# Generating public/private rsa key pair.
# Enter file in which to save the key (C:\Users\sa/.ssh/id_rsa):  
# Enter passphrase (empty for no passphrase):
# Enter same passphrase again:
# Your identification has been saved in C:\Users\sa/.ssh/id_rsa.
# Your public key has been saved in C:\Users\sa/.ssh/id_rsa.pub.
# The key fingerprint is:
```

```bash
ssh -T git@github.com
# -T ：不分配 TTY 只做代理用
```

