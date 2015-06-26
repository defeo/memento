---
layout: note
tags: web, crypto
---

Probably nothing revolutionary (isn't it what N.G. is going to do next
year?). We want a way to store our pictures on
dropbox/gdrive/etc. without the sp putting his nose into our vacation
pictures.

1. Define a data format for distributing (symmetrically) encrypted
   data together with metadata explaining where to get the decryption
   key. Possible fields:
   
   - Key server (URL): a https URL where to get the decryption key.
   - Realm (path): a path describing which key from the server is
     needed. To each realm must correspond a single key, and it is
     sane to ask that a key should be unique to a realm.
   - Content type: you get it.
   - Encrypted stream (binary, optional): the encrypted data.
   - Encryption type (optional): algorithm used for encryption.

2. Write the key server. It can/must authenticate users before giving
   the decryption key. It could also support user groups for ease of
   management. It must also have methods to create/modify realms.
   
   Write a Firebase version of the server (just a security policy).

3. Write the client library (in JS). It contains the functions for
   key-generation, encryption/decryption, fetching, caching and
   uploading keys.

4. Write a browser-based management interface.

And, obviously, the fun part: survey the crypto literature on
predicate encryption, and support multiple realms. E.g. a file could
be decrypted by ANY realm-key, or if ALL realm-keys are available,
etc. (how do you make a NOT?).

