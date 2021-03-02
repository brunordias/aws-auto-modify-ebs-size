## Auto Modify EBS Size
Script para realizar incremento automático do tamanho do EBS de uma instância EC2.

Contempla partições EXT4 ou XFS.

Verifique alterações de versões.

# Requisitos
Ubuntu 16.04 ou superior

Pacotes do Sistema Operacional:

 * `awscli`
 * `jq`
 * `bc`

EC2 deve conter `IAM Role` com a seguinte policy:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstances",
                "ec2:ModifyVolume"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

# Configuração
Copie o script para `/usr/local/sbin/` e torne-o executável.

# Parâmetros

 * `-d` | path do device Linux.
 * `-a` | path do device AWS.
 * `-l` | limite de tamanho de utilização do disco para realizar o resize
 * `-r` | região da AWS onde está a EC2.

# Uso
```
$ auto-modify-ebs-size -d /dev/device_linux -a /dev/device_aws -l limite -r aws_region
```

# Exemplo
```
$ auto-modify-ebs-size -d /dev/nvme0n1p1 -a /dev/sda1 -l 85 -r us-east-1
```

# Exemplo Cron
```
SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
15 * * * * root auto-modify-ebs-size -d /dev/nvme0n1p1 -a /dev/sda1 -l 85 -r us-east-1 >> /var/log/auto-modify-ebs-size.log
```
## Ubuntu 20.04
```
SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
15 * * * * root auto-modify-ebs-size -d /dev/root -a /dev/sda1 -l 85 -r us-east-1 >> /var/log/auto-modify-ebs-size.log
```