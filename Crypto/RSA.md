# RSA

RSA is a cryptosystem first described in 1977, and is one of the most famous public-key cryptosystems.

It is used for 2 things:

- Encrypting a message with a public key. This can then be decrypted with a private key.
- Signing a message digitally allows it to be verified and makes sure it hasn't been tampered with.

The core of RSA is that there are things that are very easy to do one way, and very very challenging to do the other way. The example that RSA uses is multiplying prime numbers together. Take the two prime numbers 97 and 83. We can multiply these two numbers very easily using a calculator or pen and paper, giving 8051. However, if I were to give you the number 2059, and asked you to find the two primes which multiply to give it, it is much harder to do this and takes a lot longer to do so.
