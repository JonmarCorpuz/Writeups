
# Task 2

```Bash
┌──(kali㉿kali)-[~]
└─$ echo 'VEhNe2p1NTdfZDNjMGQzXzdoM19iNDUzfQ==' > tmp.txt
```
```Bash
┌──(kali㉿kali)-[~]
└─$ base64 --decode tmp.txt 
THM{ju57_d3c0d3_7h3_b453}
```

# Task 3

```Bash
┌──(kali㉿kali)-[~]
└─$ exiftool ~/Downloads/Findme.jpg 
ExifTool Version Number         : 12.49
File Name                       : Findme.jpg
Directory                       : /home/kali/Downloads
File Size                       : 35 kB
File Modification Date/Time     : 2023:08:04 20:52:46-04:00
File Access Date/Time           : 2023:08:04 20:52:46-04:00
File Inode Change Date/Time     : 2023:08:04 20:52:47-04:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
X Resolution                    : 96
Y Resolution                    : 96
Exif Byte Order                 : Big-endian (Motorola, MM)
Resolution Unit                 : inches
Y Cb Cr Positioning             : Centered
Exif Version                    : 0231
Components Configuration        : Y, Cb, Cr, -
Flashpix Version                : 0100
Owner Name                      : THM{3x1f_0r_3x17}
Comment                         : CREATOR: gd-jpeg v1.0 (using IJG JPEG v62), quality = 60.
Image Width                     : 800
Image Height                    : 480
Encoding Process                : Progressive DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 800x480
Megapixels                      : 0.384
```

# Task 4

```Bash
┌──(kali㉿kali)-[~]
└─$ steghide --info ~/Downloads/Extinction.jpg 
"Extinction.jpg":
  format: jpeg
  capacity: 1.3 KB
Try to get information about embedded data ? (y/n) y
Enter passphrase: 
  embedded file "Final_message.txt":
    size: 79.0 Byte
    encrypted: rijndael-128, cbc
    compressed: yes
```
```Bash
┌──(kali㉿kali)-[~]
└─$ steghide extract -sf ~/Downloads/Extinction.jpg
Enter passphrase: 
wrote extracted data to "Final_message.txt".
```
```Bash
┌──(kali㉿kali)-[~]
└─$ cat Final_message.txt 
It going to be over soon. Sleep my child.

THM{500n3r_0r_l473r_17_15_0ur_7urn}
```

# Task 5

![]()

# Task 6

![]()

![]()

![]()

# Task 7

```Bash
┌──(kali㉿kali)-[~]
└─$ strings "THM{}" ~/Downloads/hello.hello
strings: 'THM{}': No such file
/lib64/ld-linux-x86-64.so.2
libc.so.6
puts
printf
__cxa_finalize
__libc_start_main
GLIBC_2.2.5
_ITM_deregisterTMCloneTable
__gmon_start__
_ITM_registerTMCloneTable
u/UH
[]A\A]A^A_
THM{345y_f1nd_345y_60}
Hello there, wish you have a nice day
;*3$"
GCC: (Debian 9.2.1-21) 9.2.1 20191130
crtstuff.c
deregister_tm_clones
__do_global_dtors_aux
completed.7447
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
hello.c
__FRAME_END__
__init_array_end
_DYNAMIC
__init_array_start
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
skip
_ITM_deregisterTMCloneTable
puts@@GLIBC_2.2.5
_edata
printf@@GLIBC_2.2.5
__libc_start_main@@GLIBC_2.2.5
__data_start
__gmon_start__
__dso_handle
_IO_stdin_used
__libc_csu_init
__bss_start
main
__TMC_END__
_ITM_registerTMCloneTable
__cxa_finalize@@GLIBC_2.2.5
.symtab
.strtab
.shstrtab
.interp
.note.gnu.build-id
.note.ABI-tag
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rela.dyn
.rela.plt
.init
.plt.got
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.init_array
.fini_array
.dynamic
.got.plt
.data
.bss
.comment
```

# Task 8

```Bash
┌──(kali㉿kali)-[~]
└─$ echo '3agrSy1CewF9v8ukcSkPSYm3oKUoByUpKG4L' > tmp.txt
```
```Bash                                                                                                                                    
┌──(kali㉿kali)-[~]
└─$ base58 --decode tmp.txt
THM{17_h45_l3553r_l3773r5}
```

# Task 9

![]()

![]()

# Task 10

![]()

# Task 11

```Bash
┌──(kali㉿kali)-[~]
└─$ xxd ~/Downloads/spoil.png | head
00000000: 2333 445f 0d0a 1a0a 0000 000d 4948 4452  #3D_........IHDR
00000010: 0000 0320 0000 0320 0806 0000 00db 7006  ... ... ......p.
00000020: 6800 0000 0173 5247 4200 aece 1ce9 0000  h....sRGB.......
00000030: 0009 7048 5973 0000 0ec4 0000 0ec4 0195  ..pHYs..........
00000040: 2b0e 1b00 0020 0049 4441 5478 9cec dd79  +.... .IDATx...y
00000050: 9c9c 559d eff1 cf79 9e5a bb7a 5f92 7477  ..U....y.Z.z_.tw
00000060: f640 4802 0920 1150 c420 bba2 88a8 805c  .@H.. .P. .....\
00000070: 1906 7c5d 64c0 79e9 752e 03ce 38e3 0e8e  ..|]d.y.u...8...
00000080: 2f75 e63a 23ea 8c0c e830 8e03 6470 c191  /u.:#....0..dp..
00000090: cd80 880c 4b20 0909 184c 42b6 4ed2 e9f4  ....K ...LB.N...
```
```Bash                                                                                                                                                                                                                         
┌──(kali㉿kali)-[~]
└─$ printf '\x89\x50\x4E\x47' | dd of=~/Downloads/spoil.png bs=4 conv=notrunc 
1+0 records in
1+0 records out
4 bytes copied, 7.1714e-05 s, 55.8 kB/s
```
```Bash                                                                                              
┌──(kali㉿kali)-[~]
└─$ xxd ~/Downloads/spoil.png | head                                         
00000000: 8950 4e47 0d0a 1a0a 0000 000d 4948 4452  .PNG........IHDR
00000010: 0000 0320 0000 0320 0806 0000 00db 7006  ... ... ......p.
00000020: 6800 0000 0173 5247 4200 aece 1ce9 0000  h....sRGB.......
00000030: 0009 7048 5973 0000 0ec4 0000 0ec4 0195  ..pHYs..........
00000040: 2b0e 1b00 0020 0049 4441 5478 9cec dd79  +.... .IDATx...y
00000050: 9c9c 559d eff1 cf79 9e5a bb7a 5f92 7477  ..U....y.Z.z_.tw
00000060: f640 4802 0920 1150 c420 bba2 88a8 805c  .@H.. .P. .....\
00000070: 1906 7c5d 64c0 79e9 752e 03ce 38e3 0e8e  ..|]d.y.u...8...
00000080: 2f75 e63a 23ea 8c0c e830 8e03 6470 c191  /u.:#....0..dp..
00000090: cd80 880c 4b20 0909 184c 42b6 4ed2 e9f4  ....K ...LB.N...                                                            
```
```Bash
┌──(kali㉿kali)-[~]
└─$ open ~/Downloads/spoil.png
```

![]()

# Task 12

```Bash
┌──(kali㉿kali)-[~]
└─$ firefox https://www.reddit.com/r/tryhackme/comments/eizxaq/new_room_coming_soon/
```

![]()

# Task 13

```Bash
┌──(kali㉿kali)-[~]
└─$ firefox 'https://www.splitbrain.org/_static/ook/'
```

![]()

![]()

![]()

# Task 14

```Bash
┌──(kali㉿kali)-[~]
└─$ python                                                               
Python 3.10.8 (main, Nov  4 2022, 09:21:25) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> s1 = "44585d6b2368737c65252166234f20626d"
>>> s2 = "1010101010101010101010101010101010"
>>> tmp = hex(int(s1, 16) ^ int(s2, 16))[2:]
>>> bytes.fromhex(tmp).decode('utf-8')
'THM{3xclu51v3_0r}'
```

# Task 15

```Bash
┌──(kali㉿kali)-[~]
└─$ binwalk ~/Downloads/hell.jpg --verbose

Scan Time:     2023-08-05 08:06:11
Target File:   /home/kali/Downloads/hell.jpg
MD5 Checksum:  70274357b4c2eb0d1500b42716e713ed
Signatures:    411

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.02
30            0x1E            TIFF image data, big-endian, offset of first image directory: 8
265845        0x40E75         Zip archive data, at least v2.0 to extract, uncompressed size: 69, name: hello_there.txt
266099        0x40F73         End of Zip archive, footer length: 22
```
```Bash
┌──(kali㉿kali)-[~]
└─$ ls ~/Downloads/_hell.jpg.extracted
40E75.zip  hello_there.txt
```
```Bash            
┌──(kali㉿kali)-[~]
└─$ cat ~/Downloads/_hell.jpg.extracted/hello_there.txt 
Thank you for extracting me, you are the best!

THM{y0u_w4lk_m3_0u7}
```

# Task 16

```Bash
┌──(kali㉿kali)-[~]
└─$ wget https://github.com/zardus/ctf-tools/blob/master/stegsolve/install
--2023-08-05 08:11:49--  https://github.com/zardus/ctf-tools/blob/master/stegsolve/install
Resolving github.com (github.com)... 140.82.114.3
Connecting to github.com (github.com)|140.82.114.3|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 11992 (12K) [text/plain]
Saving to: ‘install’

install                      100%[==============================================>]  11.71K  --.-KB/s    in 0.01s   

2023-08-05 08:11:49 (785 KB/s) - ‘install’ saved [11992/11992]
```
```Bash
┌──(kali㉿kali)-[~]
└─$ chmod +x stegsolve.jar
```
```Bash            
┌──(kali㉿kali)-[~]
└─$ mkdir bin
```
```Bash
┌──(kali㉿kali)-[~]
└─$ mv stegsolve.jar bin/
```
```Bash
┌──(kali㉿kali)-[~/bin]
└─$ java -jar stegsolve.jar

Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
```

![]()

![]()

![]()

![]()

# Task 17

![]()

![]()

![]()

![]()

# Task 18

```Bash
┌──(kali㉿kali)-[~]
└─$ firefox 'https://archive.org/web/'
```

![]()

![]()

![]()

![]()

# Task 19

```Bash
┌──(kali㉿kali)-[~]
└─$ firefox 'https://www.guballa.de/vigenere-solver'
```

# Task 20

```Bash
┌──(kali㉿kali)-[~]
└─$ firefox 'https://www.rapidtables.com/convert/number/decimal-to-hex.html'
```

![]()

![]()

![]()

# Task 21

