---
layout: post
title:  "AES encryption with pycrypto"
date:   2012-04-12
categories: python
tags: python encryption
---
{% include JB/setup %}

Why encrypting anything, when there are already hashes? Hashes work really well especially when they are properly salted for passwords, but if you use OAuth connections from Facebook, Twitter etc., passwords are becoming less important. Access Tokens are now the new way to access a users information, but we cannot simply hash them, because to access users Facebook information, we still need the token. So the only way o store it more secure is to encrypt it.

Encryption and decryption via pycrypto can be pretty tough. It is not as convenient as the <a href="https://dev.mysql.com/doc/refman/5.1/en/encryption-functions.html">AES_ENCRYPT</a> function in mysql and the documentation is worse than none. But anyway it’s Open Source and it is good to have it. It takes more effort to understand the library and if you want to have true interoperability between different plattforms and languages you need to understand it anyway.

How is that encryption working? Basically you have a key and a message. The key is some kind of password you need it to encrypt and decrypt the message. But the key has to be 16, 24 or 36 letters long not more not less and also the message has to be a multiple of 16, 24 or 32. Don’t ask why, I don’t know that too and I don’t care!-) So what we need to do is to pad our key with a filling char to get to the exact length. If you haven’t chosen a key yet I would recommend to chose an exactly 32 character long key, os you don’t have to pad it. As padding character I chose the ‘{‘ because the padding will be at the end of the text and it is most unlikely that the text will end with a ‘{‘. The same thing goes for the text. The text also needs to be a multiple of 16, so we have to pad that too.

But I chose a different way for the key. I simply hash it with MD5 and get automatically a 32 character key back. Obviously you can’t do that with the text/value to encrypt.

{% highlight python %}
#!/usr/bin/env python

from Crypto.Cipher import AES
from Crypto.Hash import MD5
from base64 import encodestring, decodestring #base64 encoding to enable json dumps

# the block size for the key and message must be 16, 24, or 32 for AES
BLOCK_SIZE = 32

# the character used for padding--with a block cipher such as AES, the value
# you encrypt must be a multiple of BLOCK_SIZE in length.  This character is
# used to ensure that your value is always a multiple of BLOCK_SIZE
PADDING = '{'

def pad(msg, block_size=BLOCK_SIZE, padding=PADDING):
    """
    *pad the text to be encrypted*
    - appends a padding character to the end of the String
    - until the string has block_size length

    """
    return msg + ((block_size - len(msg) % block_size) * padding)

def depad(msg, padding=PADDING):
    """depad the decryptet message"""
    return msg.rstrip(padding)

def getSecret(key):
    """hases the key to MD5"""
    return MD5.new(key).hexdigest()

def encrypt(key, msg):
    """encrypts the message"""
    secret = getSecret(key)
    cipher = AES.new(secret)
    return encodestring(cipher.encrypt(pad(msg)))

def decrypt(key, msg):
    """decrypts the message"""
    secret = doSecret(key)
    cipher = AES.new(secret)
    return depad((cipher.decrypt(decodestring(msg))))
{% endhighlight %}

You should not use this as a library, because there is no error handling implemented. For example what happens if the key is longer than the block_size, etc.. This script is just demo the encryption process with pycrypto.
