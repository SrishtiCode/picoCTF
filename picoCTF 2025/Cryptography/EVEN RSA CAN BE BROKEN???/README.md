# EVEN RSA CAN BE BROKEN???

## Description
This service provides you an encrypted flag. Can you decrypt it with just N & e? Connect to the program with netcat: $ nc verbal-sleep.picoctf.net 58750 The program's source code can be downloaded here.

## Solution

Given encrypt.py

```
from sys import exit
from Crypto.Util.number import bytes_to_long, inverse
from setup import get_primes

e = 65537

def gen_key(k):
    """
    Generates RSA key with k bits
    """
    p,q = get_primes(k//2)
    N = p*q
    d = inverse(e, (p-1)*(q-1))

    return ((N,e), d)

def encrypt(pubkey, m):
    N,e = pubkey
    return pow(bytes_to_long(m.encode('utf-8')), e, N)

def main(flag):
    pubkey, _privkey = gen_key(1024)
    encrypted = encrypt(pubkey, flag) 
    return (pubkey[0], encrypted)

if __name__ == "__main__":
    flag = open('flag.txt', 'r').read()
    flag = flag.strip()
    N, cypher  = main(flag)
    print("N:", N)
    print("e:", e)
    print("cyphertext:", cypher)
    exit()

```
When we run the netcat we get,
```
└─$ nc verbal-sleep.picoctf.net 58750
N: 22384959200364675592913891937216157650830504055263879143425271407098648714070871109140601047481750249140508151291703736760463252125331660031722871863984846
e: 65537
cyphertext: 1466404810906672512125686171905775071324769822620171117786180113921441456887406728263850250871737623514664581597389042447340378509016668421806725361089479
```
And we write decrypt.py

```
from sys import exit
from Cryptodome.Util.number import bytes_to_long, inverse, long_to_bytes

e = 65537
N = 22384959200364675592913891937216157650830504055263879143425271407098648714070871109140601047481750249140508151291703736760463252125331660031722871863984846
cyphertext = 1466404810906672512125686171905775071324769822620171117786180113921441456887406728263850250871737623514664581597389042447340378509016668421806725361089479

def gen_key():
    """
    Generates RSA key with k bits
    """
    p = N//2
    q = 2
    d = inverse(e, (p-1)*(q-1))

    return d

def decrypt(d,m):
	return pow(m, d, N)

if __name__ == "__main__":
	d = gen_key()
	plain=decrypt(d,cyphertext)
	print("N:",N)
	print("d:",d)
	print("plaintext:", long_to_bytes(plain))
	exit()
```

Then we run the script and get the flag

```
python3 decrypt.py
N: 22384959200364675592913891937216157650830504055263879143425271407098648714070871109140601047481750249140508151291703736760463252125331660031722871863984846
d: 6625110412894600456809959335340543538372734057218454183216324776046949658703871881051095842310141884850601436692700787800392533528373217324645813572635031
plaintext: b'picoCTF{tw0_1$_pr!m33991588e}'
```

## FLAG : picoCTF{tw0_1$_pr!m33991588e}
