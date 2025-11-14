# age Keys

Generate an age key pair locally:

```bash
age-keygen -o age.key
cat age.key
```

- Keep the **private** key (`age.key`) safe and never commit it to Git.
- Copy the public key (the line starting with `age1...`) and:
  - Put it into `.sops.yaml` under `creation_rules.age`.
  - Use it when encrypting secrets with `sops`.

To (re-)encrypt the MySQL secret:

```bash
cd apps/mysql
sops -e mysql-secret-plain.yaml > mysql-secret.enc.yaml
```
