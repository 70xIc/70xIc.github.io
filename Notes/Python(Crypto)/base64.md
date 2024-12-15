#python #encoding #Generation #Payload #script #function 



## What is base64?

#### Base64 is a encoding schema, which is most commonly used online, so binary data such as images can be easily included into HTML or CSS files.

###  In Python, after importing the base64 module with `import base64`, you can use the `base64.b64encode()` function. Remember to decode the hex first as the challenge description states.



## Usage: 

````
#!/usr/bin/python3

import sys
import base64

hex = "72bca9b68fc16ac7beeb8f849dca1d8a783e8acf9679bf9269f7bf"

a = bytes.fromhex(hex)

print(f"Hex decoded: {a}")

b = base64.b64encode(a)

print(f"Hex converted to Base64: {b}")
````

##### Output:
##### `Hex decoded: b'r\xbc\xa9\xb6\x8f\xc1j\xc7\xbe\xeb\x8f\x84\x9d\xca\x1d\x8ax>\x8a\xcf\x96y\xbf\x92i\xf7\xbf'
##### ``Hex converted to Base64: b'crypto/Base+64+Encoding+is+Web+Safe/'```