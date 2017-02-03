# Ruby and Postfix on Ubuntu 16.04 LTS [![](https://images.microbadger.com/badges/image/rdeavila/ruby-postfix.svg)](http://microbadger.com/images/rdeavila/ruby-postfix "Get your own image badge on microbadger.com") [![](https://images.microbadger.com/badges/version/rdeavila/ruby-postfix.svg)](http://microbadger.com/images/rdeavila/ruby-postfix "Get your own version badge on microbadger.com")

Run postfix with smtp authentication (sasldb) in a docker container.
TLS and OpenDKIM support are optional.

Ruby environment, version 2.4.0, included.

Work based on [catatnight/dock3er-postfix](https://github.com/catatnight/docker-postfix)
image.

## Requirement
+ Docker 1.12

## Installation
1. Build image

	```bash
	$ sudo docker pull rdeavila/ruby-postfix
	```

## Usage

1. Create postfix container with smtp authentication

```bash
$ sudo docker run -p 25:25 \
  -e maildomain=mail.example.com -e smtp_user=user:pwd \
  --name postfix -d rdeavila/ruby-postfix
# Set multiple user credentials: -e smtp_user=user1:pwd1,user2:pwd2,...,userN:pwdN
```

2. Enable OpenDKIM: save your domain key ```.private``` in ```/path/to/domainkeys```

```bash
$ sudo docker run -p 25:25 \
  -e maildomain=mail.example.com -e smtp_user=user:pwd \
  -v /path/to/domainkeys:/etc/opendkim/domainkeys \
  --name postfix -d rdeavila/ruby-postfix
```

3. Enable TLS(587): save your SSL certificates ```.key``` and ```.crt``` to  ```/path/to/certs```

```bash
$ sudo docker run -p 587:587 \
  -e maildomain=mail.example.com -e smtp_user=user:pwd \
  -v /path/to/certs:/etc/postfix/certs \
  --name postfix -d rdeavila/ruby-postfix
```

## Note
+ Login credential should be set to (`username@mail.example.com`, `password`) in Smtp Client
+ You can assign the port of MTA on the host machine to one other than 25 ([postfix how-to](http://www.postfix.org/MULTI_INSTANCE_README.html))
+ Read the reference below to find out how to generate domain keys and add public key to the domain's DNS records

## Reference
+ [Postfix SASL Howto](http://www.postfix.org/SASL_README.html)
+ [How To Install and Configure DKIM with Postfix on Debian Wheezy](https://www.digitalocean.com/community/articles/how-to-install-and-configure-dkim-with-postfix-on-debian-wheezy)
