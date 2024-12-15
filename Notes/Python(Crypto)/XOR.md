#python #encoding #function #Generation #script 


## What is XOR?

#### **XOR is a bitwise operator which returns 0 if the bits are the same, and 1 otherwise. In textbooks the XOR operator is denoted by ⊕, but in most challenges and programming languages you will see the caret `^` used instead.**


### The Python `pwntools` library has a convenient `xor()` function that can XOR together data of different types and lengths. But first, you may want to implement your own function to solve this.




## Usage:

````
from pwn import *
import time

xor2 = bytes.fromhex("0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104")

print(xor2)

format_flag = "crypto{"

FLAG = xor(xor2, "myXORkey")
print(FLAG)
````

##### Output: 
##### `b'crypto{1f_y0u_Kn0w_En0uGH_y0u_Kn0w_1t_4ll}'`