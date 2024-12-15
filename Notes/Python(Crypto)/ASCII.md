#python #function #Generation #function #script #encoding 


## How it works and what is ASCII?

#### ASCII is a 7-bit encoding standard which allows the representation of text using the integers 0-127.

###  In Python, the `chr()` function can be used to convert an ASCII (the `ord()` function does the opposite).



## Usage:

#### `Ascii = [99, 114, 121, 112, 116, 111, 123, 65, 83, 67, 73, 73, 95, 112, 114, 49, 110, 116, 52, 98, 108, 51, 125]`

````
Ascii = [99, 114, 121, 112, 116, 111, 123, 65, 83, 67, 73, 73, 95, 112, 114, 49, 110, 116, 52, 98, 108, 51, 125]

for i in Ascii:
        print(chr(i), end="")
        if not Ascii==125:
                continue
````

### Output: 
##### `crypto{ASCII_pr1nt4bl3}`