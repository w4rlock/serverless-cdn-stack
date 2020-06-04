<p align="center">
  <img alt="Aws Cdn" src="https://user-images.githubusercontent.com/621906/79572060-adf02b00-8092-11ea-9a43-76ba7f66a0a5.jpg">
</p>

[![serverless](http://public.serverless.com/badges/v3.svg)](http://www.serverless.com)
![CI](https://github.com/w4rlock/serverless-cloudfront-stack/workflows/CI/badge.svg)
[![npm version](https://badge.fury.io/js/serverless-cloudfront-stack.svg)](https://badge.fury.io/js/serverless-cloudfront-stack)
[![npm downloads](https://img.shields.io/npm/dt/serverless-cloudfront-stack.svg?style=flat)](https://www.npmjs.com/package/serverless-cloudfront-stack)
[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=278YCRJXTXLXJ)


### Installation
```bash
npm i -E serverless-cloudfront-stack
```

### Features
```yaml
S3
- Bucket for your app assets.
- Bucket access control.
- Bucket encryption support for AES256.
- Bucket versioning for your assets.
- Bucket for cdn access logs.
- Bucket Cors.
- Data rentention for access logs.

Cloud Front Distribution
- SSL support.
- Access Logs.
- Cors support.
- Cname support.

Dns Alias
- Route53 - Record for your CDN.
- ApiGateway - New Path for your CDN

Extras
- Invalidate CloudFront cache on new deploys.
- Sync your local assets folder with s3 on new deploys.
- Extends resources configuration.
```
### Diagram

<p align="center">
  <img alt="Aws Designer" width="680" height="540" src="https://user-images.githubusercontent.com/621906/79576361-881a5480-8099-11ea-83f5-f138a415a237.png">
</p>

### Usage
```yaml
# serverless.yml

plugins:
  - serverless-cloudfront-stack

custom:
  cdnStack:
    disabled: false                               # optional, disabled this plugin
    beforeSpawn: ''                               # optional, run another hook plugin

    cname: ''                                     # optional, cname for cdn.
    createInRoute53: false                        # optional, default false, create cname record
    certificate: ''                               # required, if use cname

    apigateway: ''                                # optional, use apigateway as http_proxy to cloudfront

    bucketName: ''                                # required, app bucket name
    enableCors: false                             # optional, enable cors default false
    blockPublicAccess: true                       # optional, block public acccess to s3
    bucketEncryption: false                       # optional, bucket encryption for AES256
    bucketVersioning: false                       # optional, assets object versioning (backup)

    indexPage: ''                                 # optional, default index.html
    errorPage: ''                                 # optional, default index.html
    bucketWebHosting: true                        # optional, default true

    syncLocalFolder: ''                           # optional, folder path to sync with s3
    priceClass: ''                                # optional, default 'PriceClass_100'

    logging:                                      # optional tag
      bucketName: ''                              # required, logs bucket name
      preffix: ''                                 # optional, default 'Access/'
      retentionDays: 7                            # optional, default 21 days

```

### Simple Example For Static Web App with Route53
```yaml
# serverless.yml

custom:
  cdnStack:
    cname: 'cdn.dev.domain.com'
    createInRoute53: true                         # assumptions: exist hosted zone "dev.domain.com"
    certificate: '*.domain.com'                   # assumptions: exist acm cert "*.domain.com"

    bucketName: 'static-html-web-app'
    blockPublicAccess: true
    bucketWebHosting: true

    syncLocalFolder: './dist'


```
### Simple Example For Static Web App with ApiGateway (multiple front approach)
```yaml
# serverless.yml
# Your front-app will be accessed through api gateway
# Assumptions: should be exists the apigateway and stage

custom:
  cdnStack:
    apigateway: apigateway-name:stage-dev/new-front-app

    bucketName: 'static-html-web-app'
    blockPublicAccess: true
    bucketWebHosting: true

    syncLocalFolder: './dist'


```

### Simple Example for Front Resources (webfonts, images, ...)
```yaml
# serverless.yml

custom:
  cdnStack:
    cname: 'cdn.dev.domain.com'
    createInRoute53: true                         # assumptions: exist hosted zone "dev.domain.com"
    certificate: '*.domain.com'                   # assumptions: exist acm cert "*.domain.com"

    bucketName: 'assets-front-resources'
    enableCors: true
    blockPublicAccess: true
    bucketWebHosting: false

    syncLocalFolder: './dist'


# psss. check full example in serverless.example.yml file.
```

### Extends
```yaml
# serverless.yml

resources:
  Resources:
    StaticWebSiteBucket:
      Properties:
        BucketName: override bucket name
        ...

    DnsRecord:
      Properties:
        Comment: override comment
        ...

    CloudFrontDistribution:
      ...

    AccessLogsBucket:
      ...

```

## Donation
Donate helps me to continue adding new features or bugs fix..
If this project help you reduce time to develop, you can donate me a IPA :beer:...

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=278YCRJXTXLXJ)

