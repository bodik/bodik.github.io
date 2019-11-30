# misc notes

## Firefox slow on TLS, Cleanup cert9.db

when working on devops ci environments with self-signed certificates, sometimes is firefox getting slow on tls connections to the tester server. I've lost the original story, but the point is that cert8.db/cert9.db is having too many accepted certificates for such name, so it's very problematic to find a trusted root ... solution be as simple as cleaning up the database from the old certs ...

```
certutil -L -d sql:/home/xxx/.mozilla/firefox/ymz3xw71.profilename
certutil -d sql:/home/xxx/.mozilla/firefox/ymz3xw71.profilename/ -D -n tester.example.org
```
