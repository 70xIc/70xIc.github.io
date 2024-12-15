#function #Generation #python #script 



## GCD

#### The Greatest Common Divisor (GCD), sometimes known as the highest common factor, is the largest number which divides two positive integers **(a,b)**.

### `import math` and ``math.gcd(a,b)``


## Usage:

````                                                
import math

a = 66528
b = 52920

gcd = math.gcd(a, b)

print(f"It is: {gcd}")
````
##### Output:
##### `It is: 1512`



## Extended GCD

#### The extended Euclidean algorithm is an efficient way to find integers u,v such that.

### `from sympy import *` and `gcd, {var1}, {var2} = gcdex(p,q)`



## Usage:

````                                                
import math
from sympy import *



p = 26513
q = 32321



gcd, a, v = gcdex(p,q)


print(f"GCD is: {gcd}\n\na is = {a}\nv is = {v}\n\nFlag: crypto""{", min(a,v),"}")
````
##### Output:
````
GCD is: 10245

a is = -8404
v is = 1

Flag: crypto{ -8404 }
````




## Modular Arithmetic

#### Modulating is like how many times can the number be divided.

### `import math` and `(**), %`


## Usage:

````                                                
import math
from sympy import *
from pwn import *
from Crypto.Util.number import *
import codecs



p = (273246787654 ** 65536) % 65537


print(p)

````
##### Output:
##### `1`




## Modular Inverting

#### Adding and multiplying elements, and always obtain another element of the field.

### `for d in (255)`

## Usage:
### ``3 ⋅ d ≡ 1 mod 13?``

````
import math

for i in range(255):
        if (3 * i) % 13 == 1:
                print(i)
                break
````
