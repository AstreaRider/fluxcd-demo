# Manage Kubernetes Secrets with SOPS
## Prerequisites
- Install [gnupg](https://www.gnupg.org/)
- Install [SOPS](https://github.com/getsops/sops)
## Import Public Key
You can use public key in `apps/secrets-management/.sops.pub.asc`

Then run this command to import public key
```
gpg --import ./apps/secrets-management/.sops.pub.asc
```

Copy gpg key
````
gpg: key C3CC931F214DCEA0: "k8s-endpoint (flux secrets)" not changed
gpg: Total number processed: 1
gpg:              unchanged: 1
````

Export gpg key
```
export KEY_FP=C3CC931F214DCEA0
```

## Encrypt Secrets Manifest
Create .sops.yaml file
```
cat <<EOF > ./<path-to-secret-manifest>/.sops.yaml
creation_rules:
  - path_regex: .*.yml
    encrypted_regex: ^(data|stringData)$
    pgp: ${KEY_FP}
EOF
```
Then you can encrypt a secret manifest file by run this command
```
sops -e <path-to-secret-manifest>.yml > secret-encrypted.yml
```