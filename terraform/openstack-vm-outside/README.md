### Terraform Example
- init terraform
```
ssh-copy-id vagrant@10.101.102.250
terraform init
```

- apply with spesific tfvars
```
terraform apply -var-file="variables/internet.tfvars"
```
