# JWT
**Javascript Web Tokens** - A method of storing data on the client side so that it is readable, but not writable by the end user. The format is 3 JSON objects, joined by a `.`; first is a header (containing metadata about the token, such as the signing algorithm). The next segment is the data, which contains the actual data held. The final portion is the signature, which consists of the header and the data passed through a cryptographic function. The idea is that users cannot generate a correctly signed signature, without knowing some server secret.

## None Algorithm
In some implementations of JWT, it is possible to set the algorithm to 'None'. This means that passing an empty signature will result in maliciously crafted data passing a signature check.

## HS/RS256 confusion
If a webapp uses RS256, the data is signed using an RSA private key, then checked using the corresponding public key. The HS256 algorithm uses a single secret to encrypt and decrypt. If a webapp does not force RS256, the header can be switched to HS256. This will result in the public key being used as the 'secret'. If the public key can be obtained, it can be used to sign a message, which will pass checks on the server.
