# flags are stepic

## Description

A group of underground hackers might be using this legit site to communicate. Use your forensic techniques to uncover their message

Additional details will be available after launching your challenge instance.

# SOLUTION

When we go to the webpage, there are many flags are there. If we see the  { name: "Upanzi, Republic The",img: "flags/upz.png", style:"width: 120px!important; height: 90px!important;" }, it is the longest one
It have some hidden information in this. Also the stepic is a library of python use for Steganography in Images.

So we download the image 

And run the script
```
$ subl solve.py
```
The python code
```
from PIL import Image
import stepic

im1 = Image.open('upz.png')
s = stepic.decode(im1) 
print(s)
```
```
─$ python3 solve.py
/usr/lib/python3/dist-packages/PIL/Image.py:3402: DecompressionBombWarning: Image size (150658990 pixels) exceeds limit of 89478485 pixels, could be decompression bomb DOS attack.
  warnings.warn(
picoCTF{fl4g_h45_fl4g16aa94cf}
```

And we get the flag

## FLAG - picoCTF{fl4g_h45_fl4g16aa94cf}
