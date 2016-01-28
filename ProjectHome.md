[curve25519](http://cr.yp.to/ecdh.html) is an elliptic curve, developed by [Dan Bernstein](http://cr.yp.to/djb.html), for fast [Diffie-Hellman](http://en.wikipedia.org/wiki/Diffie-Hellman) key agreement. DJB's [original implementation](http://cr.yp.to/ecdh.html) was written in a language of his own devising called [qhasm](http://cr.yp.to/qhasm.html). The original qhasm source isn't available, only the x86 32-bit assembly output.

Since many x86 systems are now 64-bit, and portability is important, this project provides alternative implementations for other platforms.

| **Implementation** | **Platform** | **Author** | **32-bit speed** | **64-bit speed** | **Constant time** |
|:-------------------|:-------------|:-----------|:-----------------|:-----------------|:------------------|
| `curve25519`       | x86 32-bit   | `djb`      | 265µs            | N/A              | yes               |
| `curve25519-donna-c64` | 64-bit C     | `agl`      | N/A              | 215µs            | yes               |
| `curve25591-donna` | Portable C   | `agl`      | 2179µs           | 610µs            | yes               |

(All tests run on a 2.33GHz Intel Core2)

Obviously, 32-bit, non x86 platforms are currently underserved. If there's a demand for a faster implementation, a different limb pattern in the C code would probably work very well. Contact agl AT imperialviolet DOT org if you have a need for such code.

## Usage ##

The usage is exactly the same as djb's code (as described at
[http://cr.yp.to/ecdh.html](http://cr.yp.to/ecdh.html)) except that the function is called `curve25519_donna`.

To generate a private key, generate 32 random bytes and:

```
mysecret[0] &= 248;
mysecret[31] &= 127;
mysecret[31] |= 64;
```

To generate the public key, just do

```
static const uint8_t basepoint[32] = {9};
curve25519_donna(mypublic, mysecret, basepoint);
```

To generate a shared key do:

```
uint8_t shared_key[32];
curve25519_donna(shared_key, mysecret, theirpublic);
```

And hash the shared\_key with a cryptographic hash function before using.

For more information, see [djb's page](http://cr.yp.to/ecdh.html)

## Download ##

`curve25519` (DJB's implementation): [curve25519-20050915.tar.gz](http://cr.yp.to/ecdh/curve25519-20050915.tar.gz)

To get the source for donna, clone the [git repository](http://github.com/agl/curve25519-donna/tree/master)

## Papers ##

[DJB's curve25519 paper](http://cr.yp.to/ecdh/curve25519-20060209.pdf)