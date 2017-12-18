mkdir aws 
cd aws
pipenv --python=/Users/jinlong/.pyenv/versions/3.6.3/bin/python
pipenv --python=/Users/jinlong/.pyenv/versions/2.7.14/bin/python
sed -i "" "s/python.org/douban.com/g" Pipfile


## AWS-Cli
[AWS-Cli](https://github.com/aws/aws-cli)
AWS-Cli 不支持Python3(2017.12.11)
pipenv install awscli

aws configure

输入[Identity and Access Management](https://console.aws.amazon.com/iam/home#/home)

AWSAccessKeyId
AWSSecretKey


~/.aws/credentials
[default]
aws_access_key_id=foo
aws_secret_access_key=bar

[testing]
aws_access_key_id=foo
aws_secret_access_key=bar


## Boto 3
[Boto 3](https://boto3.readthedocs.io/en/latest/index.html)

pipenv install boto3