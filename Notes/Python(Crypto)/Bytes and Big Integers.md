#python #encoding #python #Generation #function #script 


## What is Bytes and Big Integers?

#### Its he most common way is to take the ordinal bytes of the message, convert them into hexadecimal, and concatenate. This can be interpreted as a base-16/hexadecimal number, and also represented in base-10/decimal.

### Python's PyCryptodome library implements this with the methods `bytes_to_long()` and `long_to_bytes()`. You will first have to install PyCryptodome and import it with `from Crypto.Util.number import`


## Usage: 

````
from Crypto.Util.number import *

inte = 11515195063862318899931685488813747395775516287289682636499965282714637259206269

print(bytes(long_to_bytes(inte, 16)).decode("utf-8")
````

##### Output:
##### `crypto{3nc0d1n6_4ll_7h3_w4y_d0wn}`