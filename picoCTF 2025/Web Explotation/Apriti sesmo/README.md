# Apriti sesmo

## Description
I found a web app that claims to be impossible to hack! Try it here!

### SOLUTION

When we visit the website and view the source page.
`view-source:http://verbal-sleep.picoctf.net:57184/impossibleLogin.php`

Then we add `~`
`view-source:http://verbal-sleep.picoctf.net:57184/impossibleLogin.php~`

It first decodes the parameter names using base64_decode
base64_decode(“\144\130\x4e\154\x63\155\x35\x68\142\127\125\x3d”) gives username
base64_decode(“\143\x48\x64\x6b”) gives password

So it may seem that we need to use SHA-1 collision, and find two parameters which are equal and have same SHA-1, this technically possible, but for this challenge it seems like overcomplication.

The variables

$yuf85e0677 = $_POST[‘username’]
$rs35c246d5 = $_POST[‘pwd’]

Will store arrays instead of strings

Calling the sha1 function with an array in PHP triggers a warning and returns null
This means both calls

sha1($yuf85e0677)
sha1($rs35c246d5)

Will return null making the condition

if(sha1($yuf85e0677) === sha1($rs35c246d5))

Evaluate as null === null which is true

By submitting data as arrays we’re bypassing the SHA1 collision check without generating a hash collision.
And we get the answer.

Now, we start the **burp suite**

Send the request on repeater
username[]=123&pwd[]=456
When sending data this way

## FLAG - picoCTF{w3Ll_d3sErV3d_Ch4mp_9c79e5f6}
