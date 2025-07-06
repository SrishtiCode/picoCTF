# Ph4nt0m 1ntrud3r

## Description

A digital ghost has breached my defenses, and my sensitive data has been stolen! 😱💻 Your mission is to uncover how this phantom intruder infiltrated my system and retrieve the hidden flag. To solve this challenge, you'll need to analyze the provided PCAP file and track down the attack method. The attacker has cleverly concealed his moves in well timely manner. Dive into the network traffic, apply the right filters and show off your forensic prowess and unmask the digital intruder! Find the PCAP file here Network Traffic PCAP file and try to get the flag. 

## SOLUTION

When we open the file in Wireshark. It show that in the when we study in each tcp section there is a base46 code. When we decode them we get the text.

```
ezF0X3c0cw==     {1t_w4s
cGljb0NURg==     picoCTF
bnRfdGg0dA==     nt_th4t
YmhfNHJfOQ==     bh_4r_9
fQ==             }
XzM0c3lfdA==     _34sy_t
NTlmNTBkMw==     59f50d3
```

After arranging them according to the time we get,

```
cGljb0NURg==    picoCTF
ezF0X3c0cw==    {1t_w4s
bnRfdGg0dA==    nt_th4t
XzM0c3lfdA==    _34sy_t
YmhfNHJfOQ==    bh_4r_9
NTlmNTBkMw==    59f50d3
fQ==            }
```

## FLAG - picoCTF{1t_w4snt_th4t_34sy_tbh_4r_959f50d3}


