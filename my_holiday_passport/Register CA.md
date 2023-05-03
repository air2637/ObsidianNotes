## create key

create user - my_holiday_passport_route53_dns_verification
	1. with access key: AKIA4TLUZBZQ46VBS36U,
	2. secret access key: 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



```bash
# install Acme script
curl https://get.acme.sh | sh

CF_Domain="second-holidaypass.air2637.life"
CF_Email="zhengwei1551@163.com"
certPath=/root/cert
mkdir $certPath

export AWS_ACCESS_KEY_ID="AKIA4TLUZBZQ46VBS36U"
export AWS_SECRET_ACCESS_KEY="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"

/root/.acme.sh/acme.sh --set-default-ca --server letsencrypt

~/.acme.sh/acme.sh --issue --dns dns_aws -d ${CF_Domain} -d *.${CF_Domain} --log

~/.acme.sh/acme.sh --installcert -d ${CF_Domain} -d *.${CF_Domain} --ca-file /root/cert/ca.cer \
--cert-file /root/cert/${CF_Domain}.cer --key-file /root/cert/${CF_Domain}.key \
--fullchain-file /root/cert/fullchain.cer
```