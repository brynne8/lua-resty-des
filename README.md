# lua-resty-des
**DES** and **3DES** for ngx_lua. This implementation is influenced by [lua-resty-string](https://github.com/openresty/lua-resty-string) by agentzh. These modules bind OpenSSL libary through LuaJIT FFI. The padding is **PKCS#7**, and it's compatible with **PKCS#5**.

## Usage

```lua
local des3 = require('resty.des3')
local str = require('resty.string')

local des3_cbc_with_iv = assert(des3:new('12345678abcdefgh87654321', nil,
  des3.cipher('cbc'), { iv = '12345678' } ))

local crypted = des3_cbc_with_iv:encrypt("I love you. But it's a secret.")

ngx.say('DES-CBC Encryption with PKCS7 padding and iv=12345678')
ngx.say('  HEX: ' .. str.to_hex(crypted))
ngx.say('  Base64: ' .. ngx.encode_base64(crypted))

ngx.say('\nDES-CBC Decryption')
ngx.say('  ' .. des3_cbc_with_iv:decrypt(crypted))
```

The output is

```
DES-CBC Encryption with PKCS7 padding and iv=12345678
  HEX: 894bd277d612565661aaedb1d47ded1569bf522fb66a3d627d8432b98749bd67
  Base64: iUvSd9YSVlZhqu2x1H3tFWm/Ui+2aj1ifYQyuYdJvWc=

DES-CBC Decryption
  I love you. But it's a secret.
```

DES encryption is similar, only to note that the key length is 8 and DES is not recommended to use.

## License

2-Clause BSD.