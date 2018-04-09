[acme.sh](https://github.com/Neilpang/acme.sh/wiki/%E8%AF%B4%E6%98%8E)
[DNS manual mode](https://github.com/Neilpang/acme.sh/wiki/dns-manual-mode)
 
 
 yum -y install openssl 
 
 curl https://get.acme.sh | sh
 
 ./acme.sh --issue -d quanjinlong.cn --dns \
 --yes-I-know-dns-manual-mode-enough-go-ahead-please
 
./acme.sh --renew -d quanjinlong.cn \
  --yes-I-know-dns-manual-mode-enough-go-ahead-please
  
copy/安装 证书
mkdir -p /data/www/ssl

./acme.sh --install-cert -d quanjinlong.cn \
--key-file       /data/www/ssl/quanjinlong.cn.key.pem  \
--fullchain-file /data/www/ssl/quanjinlong.cn.cert.pem \
--reloadcmd     "service nginx force-reload"


"/root/.acme.sh"/acme.sh --issue --dns dns_cx -d quanjinlong.cn -k ec-256

./acme.sh --install-cert -d quanjinlong.cn_ecc \
--key-file       /data/www/ssl/quanjinlong.cn/ecc_key.pem  \
--fullchain-file /data/www/ssl/quanjinlong.cn/ecc_cert.pem \
--reloadcmd     "service nginx force-reload"

  
## log
  
```bash
  [root@VM_8_12_centos .acme.sh]# ./acme.sh --issue -d quanjinlong.cn --dns \
>  --yes-I-know-dns-manual-mode-enough-go-ahead-please
[Thu Apr  5 09:29:42 CST 2018] Registering account
[Thu Apr  5 09:29:47 CST 2018] Registered
[Thu Apr  5 09:29:47 CST 2018] ACCOUNT_THUMBPRINT='kiwEXzVZgNIaJAK8Nhfc82QAu8MUgz5hkXiSc-1dNIw'
[Thu Apr  5 09:29:47 CST 2018] Creating domain key
[Thu Apr  5 09:29:47 CST 2018] The domain key is here: /root/.acme.sh/quanjinlong.cn/quanjinlong.cn.key
[Thu Apr  5 09:29:47 CST 2018] Single domain='quanjinlong.cn'
[Thu Apr  5 09:29:47 CST 2018] Getting domain auth token for each domain
[Thu Apr  5 09:29:47 CST 2018] Getting webroot for domain='quanjinlong.cn'
[Thu Apr  5 09:29:47 CST 2018] Getting new-authz for domain='quanjinlong.cn'
[Thu Apr  5 09:29:49 CST 2018] The new-authz request is ok.
[Thu Apr  5 09:29:49 CST 2018] Add the following TXT record:
[Thu Apr  5 09:29:49 CST 2018] Domain: '_acme-challenge.quanjinlong.cn'
[Thu Apr  5 09:29:49 CST 2018] TXT value: 'n1_OFICy66s8pQT9_Y_B6_CPtVggPaBQgILNOMx_Nfw'
[Thu Apr  5 09:29:49 CST 2018] Please be aware that you prepend _acme-challenge. before your domain
[Thu Apr  5 09:29:49 CST 2018] so the resulting subdomain will be: _acme-challenge.quanjinlong.cn
[Thu Apr  5 09:29:49 CST 2018] Please add the TXT records to the domains, and re-run with --renew.
[Thu Apr  5 09:29:49 CST 2018] Please add '--debug' or '--log' to check more details.
[Thu Apr  5 09:29:49 CST 2018] See: https://github.com/Neilpang/acme.sh/wiki/How-to-debug-acme.sh
[root@VM_8_12_centos .acme.sh]# ./acme.sh --renew -d quanjinlong.cn \
>   --yes-I-know-dns-manual-mode-enough-go-ahead-please
[Thu Apr  5 09:35:45 CST 2018] Renew: 'quanjinlong.cn'
[Thu Apr  5 09:35:47 CST 2018] Single domain='quanjinlong.cn'
[Thu Apr  5 09:35:47 CST 2018] Getting domain auth token for each domain
[Thu Apr  5 09:35:47 CST 2018] Verifying:quanjinlong.cn
[Thu Apr  5 09:35:55 CST 2018] Pending
[Thu Apr  5 09:35:59 CST 2018] Success
[Thu Apr  5 09:35:59 CST 2018] Verify finished, start to sign.
[Thu Apr  5 09:36:02 CST 2018] Cert success.
-----BEGIN CERTIFICATE-----
MIIGBzCCBO+gAwIBAgISAyvAMFcP9Xllga0gd+ZYlZpdMA0GCSqGSIb3DQEBCwUA
MEoxCzAJBgNVBAYTAlVTMRYwFAYDVQQKEw1MZXQncyBFbmNyeXB0MSMwIQYDVQQD
ExpMZXQncyBFbmNyeXB0IEF1dGhvcml0eSBYMzAeFw0xODA0MDUwMDM2MDFaFw0x
ODA3MDQwMDM2MDFaMBkxFzAVBgNVBAMTDnF1YW5qaW5sb25nLmNuMIIBIjANBgkq
hkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAo91hTopV6JCu8tdfgPka43fkDYIIYu1r
gFVbQcvU01sAVd7XkXUknzpnZCKnCEAfQCrAKyJvqHh/fqtpxkFd6K4XuGH9xJXf
PBd1PZ02NhrCVPpiKTDLsaBqn/IYBS0YzvwUSYNl5bbUjgozCRrAZDFsTzFe0lnM
CM55PyYkBPePWFiWEXXWyUvbjsBerMOa4gQrQl7HNHHLBZ7w0AJwTafz3iDEgrRg
u04NzVehre3FWwfOLcOe396NpH300N6EuGl2bo2cRldgek81CyM0A1JDl5hmFlAh
MlBgt/87PNGtv7ZpTq+/i+t/BfslVgavWbJ+fy7B4a3cBEBYWGbQEwIDAQABo4ID
FjCCAxIwDgYDVR0PAQH/BAQDAgWgMB0GA1UdJQQWMBQGCCsGAQUFBwMBBggrBgEF
BQcDAjAMBgNVHRMBAf8EAjAAMB0GA1UdDgQWBBSiboptH4fpTKM/ozBKc91kBDxB
oDAfBgNVHSMEGDAWgBSoSmpjBH3duubRObemRWXv86jsoTBvBggrBgEFBQcBAQRj
MGEwLgYIKwYBBQUHMAGGImh0dHA6Ly9vY3NwLmludC14My5sZXRzZW5jcnlwdC5v
cmcwLwYIKwYBBQUHMAKGI2h0dHA6Ly9jZXJ0LmludC14My5sZXRzZW5jcnlwdC5v
cmcvMBkGA1UdEQQSMBCCDnF1YW5qaW5sb25nLmNuMIH+BgNVHSAEgfYwgfMwCAYG
Z4EMAQIBMIHmBgsrBgEEAYLfEwEBATCB1jAmBggrBgEFBQcCARYaaHR0cDovL2Nw
cy5sZXRzZW5jcnlwdC5vcmcwgasGCCsGAQUFBwICMIGeDIGbVGhpcyBDZXJ0aWZp
Y2F0ZSBtYXkgb25seSBiZSByZWxpZWQgdXBvbiBieSBSZWx5aW5nIFBhcnRpZXMg
YW5kIG9ubHkgaW4gYWNjb3JkYW5jZSB3aXRoIHRoZSBDZXJ0aWZpY2F0ZSBQb2xp
Y3kgZm91bmQgYXQgaHR0cHM6Ly9sZXRzZW5jcnlwdC5vcmcvcmVwb3NpdG9yeS8w
ggEEBgorBgEEAdZ5AgQCBIH1BIHyAPAAdgApPFGWVMg5ZbqqUPxYB9S3b79Yeily
3KTDDPTlRUf0eAAAAWKTckIrAAAEAwBHMEUCIQDAxllHOVE/fIfd/1q61yIQb7K3
h+KvMJQm4ob+UjPqVgIgW3OtuN2FVCppgJhV1lKoQHSjgI9neRBGfwbpWyKmNAYA
dgBVgdTCFpA2AUrqC5tXPFPwwOQ4eHAlCBcvo6odBxPTDAAAAWKTckNSAAAEAwBH
MEUCIQDDhoKDEShosj+OYHWp04ZRGb2GBeknEvvQ6RpW/fB9LQIgI8bdN5X17KoH
HEsNhBGAVwPrP3TqcAQ7rYKsz38WWsUwDQYJKoZIhvcNAQELBQADggEBAJCm+iuD
g6CHchyNEPXfooa0zntBgzNKlwjTwSLzMJxKmFD1GBXkZGLZwRtRtu14SRw67dQG
dBUwoxsSRae/F9eHiQLJrqY7p4s79zU/0aHD8NAnGm9nERXPL2ehjnKPx0lgAQc5
8qORHJaRXPaOUGu+XpWBbXnOihq12Uhq4Bx5fIoR+4Kd4+oIET7nv00eeBPMJmiX
83xOxIKRvEAJqWj0W6DOymvkc1NaGp2LCMRRfHNbd2p+l2xLtbewjnhzf/V3XXGv
l9ZdDtEUPH4meQQY4xrAEgu9rTTLOpMmLSSPdNQFN8wJqJqPVHfcxMbPOqusQgGo
I4qVG2BqLb9t3FQ=
-----END CERTIFICATE-----
[Thu Apr  5 09:36:02 CST 2018] Your cert is in  /root/.acme.sh/quanjinlong.cn/quanjinlong.cn.cer
[Thu Apr  5 09:36:02 CST 2018] Your cert key is in  /root/.acme.sh/quanjinlong.cn/quanjinlong.cn.key
[Thu Apr  5 09:36:03 CST 2018] The intermediate CA cert is in  /root/.acme.sh/quanjinlong.cn/ca.cer
[Thu Apr  5 09:36:03 CST 2018] And the full chain certs is there:  /root/.acme.sh/quanjinlong.cn/fullchain.cer
[Thu Apr  5 09:36:03 CST 2018] It seems that you are using dns manual mode. please take care: The dns manual mode can not renew automatically, you must issue it again manually. You'd better use the other modes instead.
[Thu Apr  5 09:36:03 CST 2018] Call hook error.
```