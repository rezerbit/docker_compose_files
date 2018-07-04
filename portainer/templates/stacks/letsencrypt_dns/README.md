# Configs for Let's Encrypt SSL certificates

Used: https://github.com/adferrand/docker-letsencrypt-dns

## Preparation of the container

First of all, before using this container, two steps of configuration need to be done: describing all certificates to acquire and maintain, then configuring an access to your DNS zone to publish DNS challenges.

### Configuring the SSL certificates

This container uses a file which must be put at `/etc/letsencrypt/domains.conf` in the container. It is a simple text file which follows theses rules:
- each line represents a certificate,
- one line may contain several domains separated by a space,
- the first domain is the certificate main domain,
- each following domain on a line is included in the SAN of the certificate, allowing it to be used for several domains.

Let's take an example. Our domain is `example.com`, and we want:
- a certificate for `smtp.example.com`
- a certificate for `imap.example.com` + `pop.example.com`
- a certificate for `ldap.example.com`
- a wildcard certificate for any sub-domain of `example.com` and for `example.com` itself

Then the `domains.conf` will look like this:

```
smtp.example.com
imap.example.com pop.example.com
ldap.example.com
*.example.com example.com
```

You need also to provide the mail which will be used to register your account on Let's Encrypt. Set the environment variable `LETSENCRYPT_USER_MAIL (default: noreply@example.com)` in the container for this purpose.

_NB: For a wildcard certificate, specifying a sub-domain already covered by the wildcard will raise an error during Certbot certificate generation (eg. `test.example.com` cannot be put on the same line than `*.example.com`)._
