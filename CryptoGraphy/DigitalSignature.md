# Digital Signature

Encryption is basically converting normal text to cipher text and Decription is to convert cipher text to Normal text.<br>

Now, for encryption and decryption, we need key, which will help in this conversion.<br>

Key can be of two types-><br>
1. Symmetric Key -> Symmetric key is basically whe the encryptor and the decryptor use same key, which is Symmetric Cryptography. Now the issue is this Key needs to be share beforehand as if that's shared with the encrypted data, it can be hacked along with the data itself. But it's popular as it's faster. And bigger the size of the key, the security is more as well. Now if one person is communicating to multiple users, then they need to handle multiple keys, which is tough to maintain. To solve this issue Asymmetric key comes into the picture.<br>
For Symmetric Key we have AES(Advanced Encryption Standard), DES(Data Encryption Standard)<br>
   
2. Asymmetric Key -> It has two things , Public Key and Private Key. It works in combination, like if it's encrypted with public key then it should be decrypted with private key, or otherwise. The beauty is if suppose A sent a data encrypted with a public key, which is available to all, who are present in the system, may be it's saved in a DB, but when this data is sent to B, it can be decrypted with the private key only B owns, so, even if hacker H can access the encrypted data and public key. It can't be decrypted without the private key B owns, which B ONLY has.<br>
For Symmetric Key we have RSA(Rivest, Shamir, Adlemen), ECC(Eliptic Curve Cryptography).<br>


Now, here comes another issue, that is , Suppose A is sending an encrypted data encrypted with a public key , which is available to everyone. If hacker H comes in between and he replaced the original data encrypted with the same public key as it's available to everyone and sends to B, then B won't be able to understand that the data got replaced. So, even if what A sent, that can't be consumed by H, but he still can change the actual information.<br>
Now if we thing the opposite, like suppose A is sending data to B and A encrypted it with the private key of A, then that can be opened bu A's public key only. in that case, even if H replaces the whole thing and encrypt with his private key, when B won't be able to decrypt it with A's public key, it will be able to understand that something is not right. but again it's not full proof as H still can decrypt and see the data.

For this, we can think of double encryption, suppose, A is sending the data, first encrypted by B's public key, then encrypted by A'a private key, So when B will decrypt, first it will use A'a public key, which will ensure that it has come from A and then decrypt using B's own private key. 
To solve this Digital Signature came into the picture.<br>


