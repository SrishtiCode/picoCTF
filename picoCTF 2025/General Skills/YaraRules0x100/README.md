# YaraRules0x100

## Description

Dear Threat Intelligence Analyst, Quick heads up - we stumbled upon a shady executable file on one of our employee's Windows PCs.
Good news: the employee didn't take the bait and flagged it to our InfoSec crew. Seems like this file sneaked past our Intrusion Detection Systems, indicating a fresh threat with no matching signatures in our database. 
Can you dive into this file and whip up some YARA rules? We need to make sure we catch this thing if it pops up again. 
Thanks a bunch! The suspicious file can be downloaded here. Unzip the archive with the password picoctf

Additional details will be available after launching your challenge instance

Once you have created the YARA rule/signature, submit your rule file as follows: socat -t60 - TCP:standard-pizzas.picoctf.net:64448 < sample.txt (In the above command, modify "sample.txt" to whatever filename you use). 
When you submit your rule, it will undergo testing with various test cases. 
If it successfully passes all the test cases, you'll receive your flag.


## Solution

```
┌──(srishti㉿kali)-[~/Desktop/PicoCTF/GeneralSkills/YaraRule]
└─$ subl yaratest.txt
                                                                                                                          
┌──(srishti㉿kali)-[~/Desktop/PicoCTF/GeneralSkills/YaraRule]
└─$ wget "https://challenge-files.picoctf.net/c_standard_pizzas/8397ce84bacb935d833928aad0705946728a1ca41cf7555aae0a7e243e504e5d/suspicious.zip"
--2025-07-05 12:15:06--  https://challenge-files.picoctf.net/c_standard_pizzas/8397ce84bacb935d833928aad0705946728a1ca41cf7555aae0a7e243e504e5d/suspicious.zip
Resolving challenge-files.picoctf.net (challenge-files.picoctf.net)... 18.164.246.127, 18.164.246.112, 18.164.246.115, ...
Connecting to challenge-files.picoctf.net (challenge-files.picoctf.net)|18.164.246.127|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16506 (16K) [application/octet-stream]
Saving to: ‘suspicious.zip’

suspicious.zip                 100%[==================================================>]  16.12K  --.-KB/s    in 0s      

2025-07-05 12:15:07 (170 MB/s) - ‘suspicious.zip’ saved [16506/16506]

                                                                                                                          
┌──(srishti㉿kali)-[~/Desktop/PicoCTF/GeneralSkills/YaraRule]
└─$ ls
suspicious.zip
                                                                                                                          
┌──(srishti㉿kali)-[~/Desktop/PicoCTF/GeneralSkills/YaraRule]
└─$ unzip suspicious.zip                                  
Archive:  suspicious.zip
[suspicious.zip] suspicious.exe password: 
  inflating: suspicious.exe          
                                                                                                                          
┌──(srishti㉿kali)-[~/Desktop/PicoCTF/GeneralSkills/YaraRule]
└─$ ls
suspicious.exe  suspicious.zip
                                                                                                                          
┌──(srishti㉿kali)-[~/Desktop/PicoCTF/GeneralSkills/YaraRule]
└─$ file suspicious.exe
suspicious.exe: PE32 executable for MS Windows 6.00 (GUI), Intel i386, UPX compressed, 3 sections
                                                                                                                          
┌──(srishti㉿kali)-[~/Desktop/PicoCTF/GeneralSkills/YaraRule]
└─$ ls -la                    
total 56
drwxrwxr-x 2 srishti srishti  4096 Jul  5 12:15 .
drwxrwxr-x 6 srishti srishti  4096 Jul  5 12:11 ..
-rwxr-xr-x 1 srishti srishti 26624 Mar  6 08:57 suspicious.exe
-rw-rw-r-- 1 srishti srishti 16506 Mar  7 02:00 suspicious.zip
                                                                                                                          
┌──(srishti㉿kali)-[~/Desktop/PicoCTF/GeneralSkills/YaraRule]
└─$ upx -o suspicious_dec.exe -d suspicious.exe           
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2024
UPX 4.2.4       Markus Oberhumer, Laszlo Molnar & John Reiser    May 9th 2024

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
     40960 <-     26624   65.00%    win32/pe     suspicious_dec.exe

Unpacked 1 file.
                                                                                                                          
┌──(srishti㉿kali)-[~/Desktop/PicoCTF/GeneralSkills/YaraRule]
└─$ strings suspicious.exe | grep less             
                                                                                                                          
┌──(srishti㉿kali)-[~/Desktop/PicoCTF/GeneralSkills/YaraRule]
└─$ strings suspicious.exe | less     

zsh: done       strings suspicious.exe | 
zsh: suspended  less
                                                                                                                          
┌──(srishti㉿kali)-[~/Desktop/PicoCTF/GeneralSkills/YaraRule]
└─$ strings suspicious_dec.exe | less

zsh: done       strings suspicious_dec.exe | 
zsh: suspended  less

```

The yaratest.txt file

```
rule pico {
    meta:
        description = "pico"

    strings:
        $upx1 = "UPX0" ascii
        $upx2 = "UPX1" ascii
        $upx3 = "UPX!" ascii


        $mal_str1 = "Cr/p0" ascii wide
        $mal_str2 = "3'+8U" ascii wide
        $mal_str3 = "CHjM5" ascii wide
        $mal_str4 = "t%j@" ascii wide
        $mal_str5 = "l@Y2" ascii wide
        $mal_str6 = "b3`5d5l5" ascii wide
        $mal_str7 = "NtQueryInformationProcess" ascii wide
        $mal_str10 = "DebugActiveProcess" ascii wide
        

        $api_critical1 = "CreateToolhelp32Snapshot" ascii
        $api_critical2 = "Process32FirstW" ascii
        $api_critical3 = "Process32NextW" ascii
        $api_critical4 = "VirtualAllocEx" ascii
        $api_critical5 = "WriteProcessMemory" ascii
        $api_critical6 = "SetThreadContext" ascii
        $api_critical7 = "ResumeThread" ascii


        $hex1 = { 25 64 64 64 6C 21 30 } // %dddl!0
        $hex2 = { 62 33 60 35 64 35 6C 35 } // b3`5d5l5
        $hex3 = { 6E 74 51 75 65 72 79 49 6E 66 6F 72 6D 61 74 69 6F 6E 50 72 6F 63 65 73 73 } // NtQueryInformationProcess
        

        $sect1 = ".text" ascii
        $sect2 = ".rdata" ascii
        $sect3 = ".data" ascii
        $sect4 = ".rsrc" ascii
        $sect5 = ".reloc" ascii
        $sect6 = ".upx" ascii

    condition:
        uint16(0) == 0x5A4D and  
        filesize < 100MB and  
        (
           
            (
                (all of ($api_critical*)) and 
                (1 of ($sect*))
            ) or
            
            
            (
                (1 of ($upx*) and 1 of ($mal_str*))
            ) or

            
            (
                2 of ($mal_str*)
            ) or

            
            (
                1 of ($hex*)
            )
        )
}

```

```
┌──(srishti㉿kali)-[~/Desktop/PicoCTF/GeneralSkills/YaraRule]
└─$ socat -t60 - TCP:standard-pizzas.picoctf.net:57063 < yaratest.txt
:::::

Status: Passed
Congrats! Here is your flag: picoCTF{yara_rul35_r0ckzzz_b2b2ee8c}

:::::

```
## FLAG - picoCTF{yara_rul35_r0ckzzz_b2b2ee8c}

# References
https://yara.readthedocs.io/en/latest/
