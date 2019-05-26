## How to ?
- Active your repository in travis-ci first
> There are two weibsites: the travis-ci.com and travis-ci.org

- create .travis.yml file


## Caveats

### #1 Encryption sensitive data

```bash
$ travis encrypt-file id_rsa --com

openssl aes-256-cbc -K $encrypted_15b8f41eeee9_key -iv $encrypted_15b8f41eeee9_iv -in .travis/id_rsa.enc -out ~/.ssh/id_rsa -d
```

If `--com` is used to encrypt data, we must deactive the other one i.e. repository settings in travis-ci.org. 

Designate `--com` or `--org` option leads to different result, this is why one site reports the building result being passed while another errored.

To deactive repository,  you need go to the `https://travis-ci.[com|org]/user/repository/settings`, turn off the `Build pushed branches` and `build pushed pull resquest`.

### #2 The id_rsa should be unencrypted

if the `id_rsa` is encrypted, then travis will ask `Enter passphrase for /home/travis/.ssh/id_rsa`, the following shows how encrypted id_rsa and unencrypted one look like. 

A encrypted id_rsa file:

```bash
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,255B6029A7CCA897E45F4BFFC787E0EB

+J26e0I1BVF4tsHg7fXgvGrxS57AC5rJQxqh+YxPJn3HiqIb6VO7JL6F9YCEWXDt
bHgToK/inAr0pkVX3GqbdQ18t5CQhIi7HWy0lPpBbKQkCEfiirkwle7Gn4QcKM3P
lHme1WOjyViH7iOBBe9gC9nF4YLOz0sva/8PzvxQw2tapVcJ6vuQ4k4JuhtteXfM
```

A unencrypted is_rsa file:
```
-----BEGIN RSA PRIVATE KEY-----
MIIJKQIBAAKCAgEA38JGZj/On+Wg7tX479o6a4gaZDoF7/hl/pTBVTvyzKertcms
3+f+DzU2wRTFSGPKDQNJHLm9+1hTKvs5LfCE124GCmSuxwJOV8ooTcUePn7LxUIN
dNMeQvc3Z46IWx7h0G+U4rzUWmaijwCtfW/7RDRhgbR1aSPFiB4QK2gzJ1mFyDtM
ELkTL0S1tcrwLbf9gGIRTtmnEtamCIWY7k8i91XlJ3muFlGax2pffkka82uTi8zr
```

### #3 Remove escape character \

If you use `travis encrypt-file ~/.ssh/id_rsa.travis --com --add`, travis will add the following command in `.travis.yml` for you, but it fails when travis do the CI work. 

```
openssl aes-256-cbc -K $encrypted_701e64p6uaf3_key -iv $encrypted_701e64p6uaf3_key_iv
  -in .travis/id_rsa.travis.enc -out ~\/.ssh/id_rsa -d
```

To solve this problem, the `\` must be removed, the rigth version is the following one:
```
openssl aes-256-cbc -K $encrypted_701e64p6uaf3_key -iv $encrypted_701e64p6uaf3_key_iv
  -in .travis/id_rsa.travis.enc -out ~/.ssh/id_rsa -d
```
