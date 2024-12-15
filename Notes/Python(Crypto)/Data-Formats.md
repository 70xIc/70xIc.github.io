#python #encoding #function #Generation #script 


## PEM

#### **PEM is a popular format for sending keys, certificates, and other cryptographic material. It looks like:**

````
-----BEGIN RSA PUBLIC KEY-----  
MIIBCgKC... _(a whole bunch of base64)_  
-----END RSA PUBLIC KEY-----
````


### PyCryptodome: first import the RSA module with `from Crypto.PublicKey import RSA` and you can read the key data using `RSA.import_Key()`.



## Usage: `private_key = RSA.import_key(RSA_Key)` & `d = private_key.d
`
##### Output:
##### `15682700288056331364787171045819973654991149949197959929860861228180021707316851924456205543665565810892674190059831330231436970914474774562714945620519144389785158908994181951348846017432506464163564960993784254153395406799101314760033445065193429592512349952020982932218524462341002102063435489318813316464511621736943938440710470694912336237680219746204595128959161800595216366237538296447335375818871952520026993102148328897083547184286493241191505953601668858941129790966909236941127851370202421135897091086763569884760099112291072056970636380417349019579768748054760104838790424708988260443926906673795975104689`




## DER

#### The data that gets base64-encoded is DER-encoded ASN.1 values.


## Usage:

````
from cryptography.x509 import load_der_x509_certificate
from cryptography.hazmat.backends import default_backend

with open("hello.der", "rb") as o:
        cert = load_der_x509_certificate(o.read(), default_backend())
        modulus = cert.public_key().public_numbers()
        print(modulus.e)
        print(cert.issuer)
        print(cert.not_valid_before)
        print(cert.not_valid_after)

````
### Output:

````
65537
<Name(C=JP,ST=Tokyo,L=Chuo-ku,O=Frank4DD,OU=WebCert Support,CN=Frank4DD Web CA,1.2.840.113549.1.9.1=support@frank4dd.com)>
/root/Crypto/lol.py:10: CryptographyDeprecationWarning: Properties that return a naïve datetime object have been deprecated. Please switch to not_valid_before_utc.
  print(cert.not_valid_before)
2012-08-22 05:27:41
/root/Crypto/lol.py:11: CryptographyDeprecationWarning: Properties that return a naïve datetime object have been deprecated. Please switch to not_valid_after_utc.
  print(cert.not_valid_after)
2017-08-21 05:27:41
````
