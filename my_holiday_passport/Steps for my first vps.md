1. get an ec2 instance 
2. install the package: X-ui with github reference https://github.com/vaxilu/x-ui
```bash
	apt update -y
	apt install -y curl socat
	sudo su
	wget -O - https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh | bash

whereis x-ui
/usr/local/x-ui
/usr/local/x-ui/x-ui setting -username admin -password Zw-19901023
```



### Register a domain

I registered from GoDaddy for my first domain "air2637.life"!

### Use Route 53 to manage the DNS service

1. Create a **Public Hosted zone** in Route 53, with domain name "air2637.life"
2. Get the name servers listed, e.g. 
	ns-652.awsdns-17.net
	ns-495.awsdns-61.com
	ns-1455.awsdns-53.org
	ns-1539.awsdns-00.co.uk
3. Go to GoDaddy, delete the Nameservers, and add the name servers from Route 53
4. Add a 'A' record (as it is ipv4) that maps to the public IP address of VPS

### Get SSL certificate

Register account via acme https://doc.primekey.com/ejbca/ejbca-operations/ejbca-ca-concept-guide/protocols/acme/acme-with-acme-sh


```bash
root@ip-172-31-18-229:~/.acme.sh# . acme.sh --register-account --insecure --force --email air2637@gmail.com
[Sat Apr 22 15:19:28 UTC 2023] No EAB credentials found for ZeroSSL, let's get one
[Sat Apr 22 15:19:30 UTC 2023] Registering account: https://acme.zerossl.com/v2/DV90
[Sat Apr 22 15:19:32 UTC 2023] Registered
[Sat Apr 22 15:19:32 UTC 2023] ACCOUNT_THUMBPRINT='l8eVYVxYpv5Z95RQsUpTXViIS0eaeA8i41SD7BzD8zA'
```

get certificate based on dns api

```bash
. acme.sh --issue --dns dns_aws -d air2637.life -d *.air2637.life --log
```

get error no iam permission:

```bash
[Sat Apr 22 16:26:34 UTC 2023] Unable to fetch IAM role from instance metadata
[Sat Apr 22 16:26:34 UTC 2023] You haven't specified the aws route53 api key id and and api key secret yet.
```

resolved by creating the user
https://www.cyberciti.biz/faq/route-53-lets-encrypt-wildcard-certificate-with-acme-sh/

1. create policy - "[my_holiday_passport_ssl](https://us-east-1.console.aws.amazon.com/iam/home#/policies/arn:aws:iam::393977875982:policy/my_holiday_passport_ssl)"
2. create user - my_holiday_passport_route53_dns_verification
	1. with access key: AKIAVXOXJJYHP2PEHHMK,
	2. secret access key: 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

```bash
root@ip-172-31-18-229:~/.acme.sh# export AWS_ACCESS_KEY_ID="AKIAVXOXJJYHP2PEHHMK"
root@ip-172-31-18-229:~/.acme.sh# export AWS_SECRET_ACCESS_KEY="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"
root@ip-172-31-18-229:~/.acme.sh# . acme.sh --issue --dns dns_aws -d air2637.life -d *.air2637.life --log
```

```
[Sat Apr 22 16:44:47 UTC 2023] Cert success.
-----BEGIN CERTIFICATE-----
MIIECjCCA5GgAwIBAgIRAKOGoPn87XHKYKCmtIfngRQwCgYIKoZIzj0EAwMwSzEL
MAkGA1UEBhMCQVQxEDAOBgNVBAoTB1plcm9TU0wxKjAoBgNVBAMTIVplcm9TU0wg
RUNDIERvbWFpbiBTZWN1cmUgU2l0ZSBDQTAeFw0yMzA0MjIwMDAwMDBaFw0yMzA3
MjEyMzU5NTlaMBcxFTATBgNVBAMTDGFpcjI2MzcubGlmZTBZMBMGByqGSM49AgEG
CCqGSM49AwEHA0IABGUt8fEkBVCa8y4e7xboTPx1ZfS+o1Z+llN7qyU8sYvJeaxf
kRfj5axDo+qiwxQ0VsMm5y3/e260exEk5GD1ZTKjggKIMIIChDAfBgNVHSMEGDAW
gBQPa+ZLzjlHrvZ+kB558DCRkshfozAdBgNVHQ4EFgQUj7nzN3zelA4YWJM65t2M
m8j5P/gwDgYDVR0PAQH/BAQDAgeAMAwGA1UdEwEB/wQCMAAwHQYDVR0lBBYwFAYI
KwYBBQUHAwEGCCsGAQUFBwMCMEkGA1UdIARCMEAwNAYLKwYBBAGyMQECAk4wJTAj
BggrBgEFBQcCARYXaHR0cHM6Ly9zZWN0aWdvLmNvbS9DUFMwCAYGZ4EMAQIBMIGI
BggrBgEFBQcBAQR8MHowSwYIKwYBBQUHMAKGP2h0dHA6Ly96ZXJvc3NsLmNydC5z
ZWN0aWdvLmNvbS9aZXJvU1NMRUNDRG9tYWluU2VjdXJlU2l0ZUNBLmNydDArBggr
BgEFBQcwAYYfaHR0cDovL3plcm9zc2wub2NzcC5zZWN0aWdvLmNvbTCCAQQGCisG
AQQB1nkCBAIEgfUEgfIA8AB2AK33vvp8/xDIi509nB4+GGq0Zyldz7EMJMqFhjTr
3IKKAAABh6naUlcAAAQDAEcwRQIhAKVMjtL1yhFeSsJgPlaDIUzO+FsLDKvo/d1/
L7cJEO1WAiAaLfwlty3dw17MjoqWsBxl/uxKesQ19EuuewUX60zrkwB2AHoyjFTY
ty22IOo44FIe6YQWcDIThU070ivBOlejUutSAAABh6naUtYAAAQDAEcwRQIgLBrq
FHW5ixIETmelhfVmEmc45nZirW2bk/GrV3GFf+ACIQDiCA1ajbE6wQ8LYgTTSOJ0
glPcjVjf4maPNn8NdRNW9DAnBgNVHREEIDAeggxhaXIyNjM3LmxpZmWCDiouYWly
MjYzNy5saWZlMAoGCCqGSM49BAMDA2cAMGQCMGKbL1qhs0zHRAwxYzuzB5AUgg2g
koEWAiihOsj58/Qw6z8GXHdSEMNYqsAYLT9oxQIwAXfBrNt17SMDKshMIi9+6hKU
jHXljsyIcO1BuURg5GHZ2APqiuLY3KM6VWKOjdHg
-----END CERTIFICATE-----
[Sat Apr 22 16:44:47 UTC 2023] Your cert is in: /root/.acme.sh/air2637.life_ecc/air2637.life.cer
[Sat Apr 22 16:44:47 UTC 2023] Your cert key is in: /root/.acme.sh/air2637.life_ecc/air2637.life.key
[Sat Apr 22 16:44:47 UTC 2023] The intermediate CA cert is in: /root/.acme.sh/air2637.life_ecc/ca.cer
[Sat Apr 22 16:44:47 UTC 2023] And the full chain certs is there: /root/.acme.sh/air2637.life_ecc/fullchain.cer
```


```bash
. acme.sh --installcert -d air2637.life -d *.air2637.life --ca-file /root/cert/ca.cer --cert-file /root/cert/air2637.life.cer --key-file /root/cert/air2637.life.key --fullchain-file /root/cert/fullchain.cer
```

```bash
. acme.sh --upgrade --auto-upgrade
```


Create connection point:

vless://3f9ddd1e-9792-4d52-e1cd-3edb702b438b@holidaypassport.air2637.life:443?type=tcp&security=xtls&flow=xtls-rprx-direct#1st-passport