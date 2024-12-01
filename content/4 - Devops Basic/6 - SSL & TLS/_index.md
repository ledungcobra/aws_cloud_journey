## TLS Certificate
### SSH & Asymmetric Key
* This is used to guarantee trust between two parties
* The TLS use Asymmetric encryption to encryption data between client and server
  * It comprises of `Public Lock` and `Private Key`. When the data encryptec with `Public Lock`, it can be easily decrypted with `Private Key`.

```bash
ssh-keygen
```

It will generate a pair of key id_rsa (private key), id_rsa.pub(public key)

* To allow you or other user to access server with private key you need to copy the public key to the folder `~/.ssh/authorized_keys`.

* The ssh-keygen is meant for generate key for SSH purpose.

### Generate private and public key pair

```bash
openssl genrsa -out my-bank.key 1024
openssl rsa -in my-bank.key -pubout > mybank.pem
```

* These commands generate `my-bank.key`, `mybank.pem`
TODO