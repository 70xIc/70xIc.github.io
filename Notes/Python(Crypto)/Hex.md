#python #Generation #Generation #script #encoding #function 


## What is Hex?

#### Hexadecimal can be used in such a way to represent ASCII strings. First each letter is converted to an ordinal number according to the ASCII table.

###  In Python, the `bytes.fromhex()` function can be used to convert hex to bytes. The `.hex()` instance method can be called on byte strings to get the hex representation.


## Usage:

````                   
#!/usr/bin/python3

import sys

Hex = "63727970746f7b596f755f77696c6c5f62655f776f726b696e675f776974685f6865785f737472696e67735f615f6c6f747d"


print(bytes.fromhex(Hex))
````

##### Output: 
##### `b'crypto{You_will_be_working_with_hex_strings_a_lot}'`
