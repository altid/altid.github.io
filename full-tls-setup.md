# Full TLS Setup

*DISCLAIMER* This is an early draft, and has not been fully vetted. Use at your own risk.

Transport Layer Security, or TLS can be used to provide a secure communications channel between Altid servers and clients, and is also optional for use in many Altid services that connect to internet resources.

## Certificates, Keys

Altid uses certificate and key pairs to establish TLS connections.

In order to connect securely, we need to create a certificate and key on both the server, and the client.

Related reading:
 - https://en.wikipedia.org/wiki/Public_key_certificate
 - https://en.wikipedia.org/wiki/Diffie-Hellman_key_exchange

## Server

In many applications, self-signed certificates are unwanted, due to the client/server trust being broken. However, for most common Altid installations, the same authority manages both sides. (You!)
To use Certificate Authorities, such as Let's Encrypt is also possible, and will be covered in future versions of this guide.

For systems with openssl, generating a self-signed key/pair can be done as follows:

`openssl req -newkey rsa:4096 -nodes -sha512 -x509 -days 3650 -nodes -out -etc/ssl/certs/altid.pem -keyout /etc/ssl/private/altid.pem`

This will create the two named files, and the server will look for each under that specific name, if none is provided on the command line.

To sign client certificates, we need a Certificate Signing Request file, for example:

`openssl req -new -eky /etc/ssl/private/altid.pem -out /etc/ssl/certs/altid.csr`

### Plan9

For plan9 systems, the keys are stored in the factotum. The certificates may sit in a secstore if preferred.

Generate a server certificate + key:

```
# Make sure we create this in volatile storage
ramfs -p
cd /tmp
auth/rsagen -t 'service=tls owner=myuser' > altidserver.key
auth/rsa2x509 'C=US CN=*.myfdqn.com' altidserver.key | auth/pemencode CERTIFICATE > cert

# Move your certifiate to somewhere non-volatile
# By default, on plan9 the servers will look in the following directory:
mv cert $home/lib/altid/cert.pem

# And read the key into your factotum
cat altidserver.key > /mnt/factotum/ctl

# You'll likely want to back this key up to your secstore, or somewhere else safe
auth/secstore -p altidserver.key

# If you don't use a secstore to set up your factotum on boot, remember to read it in on boot, by adding the following to /cfg/$sysname/cpustart
cat /path/to/my/altidserver.key > /mnt/factotum/ctl

# And finally, delete the work window.
```

Refer to http://man.cat-v.org/9front/8/rsa for more information.

## Client

Here, we create and sign client certificates against our server's root certificate, created above:

```
# Create the client key
openssl genrsa -out myclient.key 4096

# Create a client sign request
openssl req -new -key myclient.key -out myclient.csr

# Sign the request to create a valid cert
# We'll make it last 1024 days
openssl x509 -req -in myclient.csr -CA /etc/ssl/certs/altid.pem -CAkey /etc/ssl/private/altid.pem -CAcreateserial -out myclient.pem -days 1024 -sha512
```

### Plan9

```
# As above, create keys in a secure directory
ramfs -p
cd /tmp

# Create the client key
auth/rsagen -t 'service=tls owner=myusername' > altidclient.key

# Create a csr from key
auth/rsa2csr 'CN=example.com' altidclient.key | auth/pemencode 'CERTIFICATE REQUEST' > altidclient.csr

# Sign your key
# At the time of this writing, I don't know how to sign a key on plan9. Any help here is appericated. I do it on a Linux machine, hopefully you have one available.
```

As a sidenote, f you have a client key in pem format, from a system with openssl you can read it into your factotum as follows

`auth/pemdecode 'PRIVATE KEY' mykey.pem | auth/asn12rsa -t 'service=tls' >/mnt/factotum/ctl`
