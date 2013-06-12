SSL Helpers
===========

These scripts can be used to generate different kinds of SSL keys and
certificates.

Usage
-----

1. Create a certificate authority::

    $ ./mkca <ca_name>
    .............++++++
    .....++++++
    writing new private key to '<ca_name>/cakey.pem'
    -----

#. Trust the CA's certificate, which will be stored at
   ``<ca_name>/cacert.crt``. On Windows and Mac, this can be done by double
   clicking the file then adding it to the list of "Trusted Root Certification
   Authorities", or similar.

#. Create an SSL certificate (where ``<cert_cn>`` will be the common name used
   on the certificate)::

    $ ./mkcert <ca_name> <cert_cn>
    --- creating key ---
    Generating a 1024 bit RSA private key
    .........++++++
    ......++++++
    writing new private key to '<cert_cn>-temp-private.pem'
    -----
    Using configuration from ./openssl.cfg
    Check that the request matches the signature
    Signature ok
    The Subject's Distinguished Name is as follows
    commonName            :T61STRING:'<cert_cn>'
    Certificate is to be certified until Jun  3 17:21:15 2031 GMT (7300 days)
    Sign the certificate? [y/n]:y
    1 out of 1 certificate requests certified, commit? [y/n]y
    Write out database with 1 new entries
    Data Base Updated
    <ca_name>/certs/<cert_cn>.pem created.

#. Use the ``certcat`` script to verify that the certificate is correct::

    $ ./certcat <ca_name>/certs/<cert_cn>.pem
    Private-Key: (1024 bit)
    modulus:
        00:b4:8c:01:20:52:a5:f1:11:d1:d1:52:bd:ec:7b:
        a6:12:d6:c6:5f:ef:ff:fc:bf:86:06:bc:51:be:8c:
        30:6b:09:15:c0:de:ab:9a:b3:85:cc:1d:a1:63:b1:
    ...
    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number: 1 (0x1)
            Signature Algorithm: sha1WithRSAEncryption
            Issuer: CN=<ca_name>
            Validity
                Not Before: Jun  8 17:21:15 2011 GMT
                Not After : Jun  3 17:21:15 2031 GMT
            Subject: CN=<cert_cn>
            Subject Public Key Info:
                Public Key Algorithm: rsaEncryption
                RSA Public Key: (1024 bit)
                    Modulus (1024 bit):
                        00:b4:8c:01:20:52:a5:f1:11:d1:d1:52:bd:ec:7b:
                        ...
                        01:b2:e3:37:12:0f:21:9e:bb
                    Exponent: 65537 (0x10001)
            X509v3 extensions:
                X509v3 Basic Constraints: 
                    CA:FALSE
                X509v3 Subject Key Identifier: 
                    36:BA:81:45:68:E0:19:41:DC:7A:B0:A8:DE:8D:82:C1:46:41:8F:EE
                X509v3 Authority Key Identifier: 
                    keyid:AD:E7:D8:50:3E:A7:9D:26:D9:92:4D:44:46:D7:88:95:CF:CC:C1:3E
                    DirName:/CN=<ca_name>
                    serial:F0:3C:D7:80:54:DD:15:9B

        Signature Algorithm: sha1WithRSAEncryption
            11:33:24:56:32:72:0c:a8:b6:b4:5d:06:02:3e:7d:2f:82:67:
            ...
            04:73

#. Copy the new key and certificate (which has been signed by ``<ca_name>``)
   into place::

    $ scp <ca_name>/certs/<cert_cn>.pem host:/etc/ssl/private/<cert_cn>.pem
