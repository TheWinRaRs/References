# Nonces



The use of nonces means that an attacker can't just replay previous comunications, as it will not be authenticated by the same chosen nonce. In the SSL/TLS handshake, the client and server exchange nonces, preventing man in the middle attacks.


Nonces can also be used in encryption, like AES-GCM mode. In this case, it is very important a nonce is not reused. The addition of a nonce makes it difficult for the attacker to gain information about the plaintext from a ciphertext, as well as making sure the same plaintext does not get mapped to the same ciphertext every time. However, if a nonce is reused, even without knowing the nonce an attacker can gain valuable information about the two plaintexts given two ciphertexts encrypted with the same nonce. In AES-GCM, if two ciphertexts are encrypted with the same nonce, then the xor of the two ciphertexts will be equal to the xor of the two plaintexts. This is also true for the IV of OFB and CTR encryption. This can give an attacker information on the plaintexts. The more ciphertexts encrypted with the same plaintext, the more information an attacker can gain.
