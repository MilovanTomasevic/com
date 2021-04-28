---
layout: post
title: How do I encrypt and decrypt a string in python?
image: https://img.electronicdesign.com/files/base/ebm/electronicdesign/image/2020/05/PROMOCryptographyHandbook_Ch5.5eceabbf11917.png?auto=format&fit=max&w=1200
description: >
  I have been looking for sometime on how to encrypt and decrypt a string. But most of it is in 2.7 and anything that is using 3.2 is not letting me print it or add it to a string.
hide_description: true
hide_image: true
comments: true
last_modified_at: 2021-04-28
categories: [stackoverflow]
tags: [python, python-3.x, encryption]
---

How do I encrypt and decrypt a string in python?

{:.no_toc}
1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## [Question](https://stackoverflow.com/questions/27335726/how-do-i-encrypt-and-decrypt-a-string-in-python) by [David](https://stackoverflow.com/users/4332658/davidhttps://stackoverflow.com/users/4332658/david)

I have been looking for sometime on how to encrypt and decrypt a string. But most of it is in 2.7 and anything that is using 3.2 is not letting me print it or add it to a string.

So what I'm trying to do is the following:

```py
mystring = "Hello stackoverflow!"
encoded = encode(mystring,"password")
print(encoded)
```
> jgAKLJK34t3g (a bunch of random letters)

```py
decoded = decode(encoded,"password")
print(decoded)
```
> Hello stackoverflow!

Is there anyway of doing this, using python 3.X and when the string is encoded it's still a string, not any other variable type.


## [Answer](https://stackoverflow.com/a/66250021/13155046) by [Milovan Tomašević](https://stackoverflow.com/users/13155046/milovan-tomašević)

### Encrypt Data

First, we need to install the cryptography library:

```sh
pip3 install cryptography
```

 - From the _cryptography_ library, we need to import `Fernet` and start generating a key - this key is required for symmetric encryption/decryption.

- To generate a key, we call the `generate_key()` method.
  - We only need to execute the above method once to generate a key.
  > You need to keep this key in a safe place. If you lose the key, you won't be able to decrypt the data that was encrypted with this key.

- Once we have generated a key, we need to load the key with `load_key()`

### Encrypt a Message

This is a three step process:

1. encode the message
2. initialize the Fernet class
3. pass the encoded message to `encrypt()` method

### Below is a full working example of encrypting a message :

```py
from cryptography.fernet import Fernet

def generate_key():
    """
    Generates a key and save it into a file
    """
    key = Fernet.generate_key()
    with open("secret.key", "wb") as key_file:
        key_file.write(key)

def load_key():
    """
    Load the previously generated key
    """
    return open("secret.key", "rb").read()

def encrypt_message(message):
    """
    Encrypts a message
    """
    key = load_key()
    encoded_message = message.encode()
    f = Fernet(key)
    encrypted_message = f.encrypt(encoded_message)

    print(encrypted_message)

if __name__ == "__main__":
    # generate_key() # execute only once 
    encrypt_message("Hello stackoverflow!")
```

output:

```sh
b'gAAAAABgLX7Zj-kn-We2BI_c9NQhEtfJEnHUVhVqtiqjkDi5dgJafj-_8QUDyeNS2zsJTdBWg6SntRJOjOM1U5mIxxsGny7IEGqpVVdHwheTnwzSBlgpb80='
```

### Decrypt Data

To decrypt the message, we just call the `decrypt()` method from the `Fernet` library. Remember, we also need to load the key as well, because the key is needed to decrypt the message.

```py
from cryptography.fernet import Fernet

def load_key():
    """
    Load the previously generated key
    """
    return open("secret.key", "rb").read()

def decrypt_message(encrypted_message):
    """
    Decrypts an encrypted message
    """
    key = load_key()
    f = Fernet(key)
    decrypted_message = f.decrypt(encrypted_message)

    print(decrypted_message.decode())

if __name__ == "__main__":
    decrypt_message(b'gAAAAABgLX7Zj-kn-We2BI_c9NQhEtfJEnHUVhVqtiqjkDi5dgJafj-_8QUDyeNS2zsJTdBWg6SntRJOjOM1U5mIxxsGny7IEGqpVVdHwheTnwzSBlgpb80=')
```

output:

```sh
Hello stackoverflow!
```
---
---

Your password is in the `secret.key` in a form similar to the password below:

```sh
B8wtXqwBA_zb2Iaz5pW8CIQIwGSYSFoBiLsVz-vTqzw=
```