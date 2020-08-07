# AES 128 CBC Library

**Implementation of AES 128 (CBC mode) for Particle Devices**

## API

### AES\_ctx

```cpp
struct AES_ctx
{
  uint8_t RoundKey[AES_keyExpSize];
  uint8_t Iv[AES_BLOCKLEN];
};
```

The `AES_ctx` structure holds the encryption/decryption context for a session. It's 192 bytes for AES 128. 

- `AES_keyExpSize` = 176 
- `AES_BLOCKLEN` = 16

### AES\_init\_ctx\_iv

```cpp
void AES_init_ctx_iv(struct AES_ctx* ctx, const uint8_t* key, const uint8_t* iv);
void AES_init_ctx(struct AES_ctx* ctx, const uint8_t* key);
void AES_ctx_set_iv(struct AES_ctx* ctx, const uint8_t* iv);
```

You typically initialize it using `AES_init_ctx_iv`. The parameters are:

- `key` (const uint8_t *) The secret private shared key between the sender and receiver. It's always 16 bytes long.
- `iv` (const uint8_t *) The initialization vector is a 16-byte random value. It assures that the given the same plaintext and key, the encrypted output is not the same. The iv should be randomly generated and does not need to be kept secret. You can transmit the iv in clear text, which is a common scenario because the other side cannot decrypt without the iv.

There are also functions to set the key and iv separately. 

### AES\_CBC\_encrypt\_buffer

```cpp
void AES_CBC_encrypt_buffer(struct AES_ctx* ctx, uint8_t* buf, uint32_t length);
```

Encrypt a block of data. `length` must be a multiple of `AES_BLOCKLEN` bytes (16). Encryption is done in place, overwriting the plain text.


### AES\_CBC\_decrypt\_buffer

```cpp
void AES_CBC_decrypt_buffer(struct AES_ctx* ctx, uint8_t* buf, uint32_t length);
```

Decrypt a block of data. `length` must be a multiple of `AES_BLOCKLEN` bytes (16). Decryption is done in place, overwriting the cypher text.

Because of the fixed block size if you are encrypting arbitrary stream data you'll need an out-of-band method of determining how much actual data there is. The AES ECB mode supports arbitrary length and is suitable for streams, except it's also insecure and is not recommended for any use.

## Original Library

https://github.com/kokke/tiny-AES-c

```
This is free and unencumbered software released into the public domain.

Anyone is free to copy, modify, publish, use, compile, sell, or
distribute this software, either in source code form or as a compiled
binary, for any purpose, commercial or non-commercial, and by any
means.

In jurisdictions that recognize copyright laws, the author or authors
of this software dedicate any and all copyright interest in the
software to the public domain. We make this dedication for the benefit
of the public at large and to the detriment of our heirs and
successors. We intend this dedication to be an overt act of
relinquishment in perpetuity of all present and future rights to this
software under copyright law.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR
OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.

For more information, please refer to <http://unlicense.org/>
```

## Revision History

### 0.0.1 (2020-08-07)

- Initial version

