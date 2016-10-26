# Reverse proxy for AWS S3 w/ basic authentication 

Forked from https://hub.docker.com/r/pottava/s3-proxy/ The difference is that this fork attempts to be more HTML5-routing friendly with rewrites to index.html whenever necessary.  

[![](https://badge.imagelayers.io/pottava/s3-proxy:latest.svg)](https://imagelayers.io/?images=pottava/s3-proxy:latest 'Get your own badge on imagelayers.io')

![circleci status](https://circleci.com/gh/pottava/aws-s3-proxy.svg?style=shield&circle-token=9bc17d02e4513df42196523a1791465e65d8ab01) 

Supported tags and respective `Dockerfile` links:  
・latest ([prod/1.5/Dockerfile](https://github.com/pottava/aws-s3-proxy/blob/master/prod/1.5/Dockerfile))

## Description

This is a reverse proxy for AWS S3, which is able to provide basic authentication as well.  
You don't need to configure a Bucket for `Website Hosting`.  

http://this-proxy.com/access/ -> s3://backet/access/index.html

([日本語はこちら](https://github.com/pottava/aws-s3-proxy/blob/master/README-ja.md))


## Usage

### 1. Set environment variables

Environment Variables     | Description                                       | Required | Default 
------------------------- | ------------------------------------------------- | -------- | -----------------
AWS_S3_BUCKET             | The `S3 bucket` to be proxied with this app.      | *        | 
AWS_S3_KEY_PREFIX         | You can configure `S3 object key` prefix.         |          | -
AWS_REGION                | The AWS `region` where the S3 bucket exists.      |          | us-east-1
AWS_ACCESS_KEY_ID         | AWS `access key` for API access.                  |          | EC2 Instance Role
AWS_SECRET_ACCESS_KEY     | AWS `secret key` for API access.                  |          | EC2 Instance Role
HTTP_CACHE_CONTROL        | Overrides S3's HTTP `Cache-Control` header.       |          | S3 Object metadata
HTTP_EXPIRES              | Overrides S3's HTTP `Expires` header.             |          | S3 Object metadata
BASIC_AUTH_USER           | User for basic authentication.                    |          | -
BASIC_AUTH_PASS           | Password for basic authentication.                |          | -
SSL_CERT_PATH             | TLS: cert.pem file path.                          |          | -
SSL_KEY_PATH              | TLS: key.pem file path.                           |          | -
APP_PORT                  | The port number to be assigned for listening.     |          | 80
ACCESS_LOG                | Send access logs to /dev/stdout.                  |          | false

### 2. Run the application

`docker run -d -p 8080:80 -e AWS_REGION -e AWS_S3_BUCKET pottava/s3-proxy`

* with basic auth:  

`docker run -d -p 8080:80 -e AWS_REGION -e AWS_S3_BUCKET -e BASIC_AUTH_USER -e BASIC_AUTH_PASS pottava/s3-proxy`

* with TLS:  

`docker run -d -p 8080:80 -e AWS_REGION -e AWS_S3_BUCKET -e SSL_CERT_PATH -e SSL_KEY_PATH pottava/s3-proxy`

* with docker-compose.yml:  

```
proxy:
  image: pottava/s3-proxy
  ports:
    - 8080:80
  environment:
    - AWS_REGION=ap-northeast-1
    - AWS_ACCESS_KEY_ID
    - AWS_SECRET_ACCESS_KEY
    - AWS_S3_BUCKET
    - BASIC_AUTH_USER=admin
    - BASIC_AUTH_PASS=password
    - ACCESS_LOG=true
  container_name: proxy
```


## Copyright and license

Code released under the [MIT license](https://github.com/pottava/aws-s3-proxy/blob/master/LICENSE).
