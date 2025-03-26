# Verify

#grep #checksum

People keep trying to trick my players with imitation flags. I want to make sure they get the real thing! I'm going to provide the SHA-256 hash and a decrypt script to help you know that my flags are legitimate. You can download the challenge files here:

- [challenge.zip](https://artifacts.picoctf.net/c_rhea/11/challenge.zip)

Additional details will be available after launching your challenge instance.

Looking at the `challenge.zip` file's contents and running `sha256sum` to find the checksum `5848768e56185707f76c1d74f34f4e03fb0573ecc1ca7b11238007226654bcd`:

```
$ find . -type f -exec sha256sum {} \; | grep 5848
5848768e56185707f76c1d74f34f4e03fb0573ecc1ca7b11238007226654bcda  ./home/ctf-player/drop-in/files/8eee7195
```

Decrypting contents with `openssl`:

```
$ openssl enc -d -aes-256-cbc -pbkdf2 -iter 100000 -salt -in "./home/ctf-player/drop-in/files/8eee7195" -k picoCTF
```

```
picoCTF{trust_but_verify_8eee7195}
```