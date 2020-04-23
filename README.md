# Sample Certificates

## Generate CA

```
openssl req -new -newkey rsa:1024 -nodes -out ca.csr -keyout ca.key
openssl x509 -trustout -signkey ca.key -days 365 -req -in ca.csr -out ca.pem
```

## Generate Client

```
openssl genrsa -out client.key 1024
openssl req -new -key client.key -out client.csr
openssl x509 -req -days 365 -in client.csr -CA ca.pem -CAkey ca.key -out client.pem
openssl pkcs12 -export -out client.pfx -inkey client.key -in client.pem
```

## Construct a Cert Chain

```
cat ca-int-cert.pem ca-cert.pem > ca-cert-chain.pem
```

## Create a CRL:

```
openssl ca -config ca.conf -gencrl -keyfile ca.key -cert ca.crt -out root.crl.pem
openssl crl -inform PEM -in root.crl.pem -outform DER -out root.crl
rm root.crl.pem
```

