# AWS for Front-End Engineers

## Deploying Single Page Apps

### Key concepts

* Hosting on AWS
* Distribute globally
* Secure with SLL
* Automatic Deployment (CI/CD)
* Dynamic response to request

### Setting up account

* Securing root account
* Least Privilege Principle

### S3

* Scalable
* Highly available
* Lifecycle management
* Versioning
* Encryption
* Security

We need to upload the static website files to a bucket, and change its properties to "Static Website Hosting".

Policy to allow public access:

```json
{
 "Version": "2012-10-17",
 "Statement": [
  {
   "Effect": "Allow",
   "Principal": "*",
   "Action": "s3:GetObject",
   "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
  }
 ]
}
```

### AWS CLI

* Improve deployment to S3.
* `aws configure`.
* `.aws/credentials` (you can have multiple).
* `aws s3 ls s3://yourbucket`.
* `aws s3 cp my-folder/ s3://mybucket/ --recursive`.

### Route 53

* Creating A (Alias) record to route from your domain to your S3 bucket.
* Create SSL certificate with AWS Certificate Manager.
* Client-side Routing error page.
* Setting up a WWW Subdomain, using a www. bucket and redirecting requests.

### CloudFront

* CDN for Web, globally distribute application.
* Use your static website domain in S3 as Origin Domain Name for CDN, not S3 bucket url.
* Redirect to HTTPS.
* Distribution Settings.
  * Add CNAMEs as Alternate Domain Names.
    * Custom SSL Certificate.
    * Default root object, `/index.html`.
* gzipping for our assets.
* Change DNS records to CloudFront distribution domain name.
* Caching S3 resources, means many fewer requests to S3, reducing costs.
* By default, CloudFront ignores request headers.
* Error pages inside CloudFront configuration.
* Invalidations to remove objects from cache before they expire (Remove everything with `*` as Object Paths).
* If you want to update files frequently, [it's recommended](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Invalidation.html) to use file versioning.

### Travis CI

* Easily configure CI/CD to S3 provider, and call CloudFront invalidation.
* Create new IAM User for CI/CD with custom Policy to deploy to S3 and invoke CloudFront invalidations.
  * S3 -> Write -> Bucket ARN.
  * CloudFront -> CreateInvalidation -> Cloudfront ARN.

### Lambda@Edge

* Deploy functions to your CloudFront edge nodes.
* Programatically modify requests and response that are going through CloudFront.
* Useful when you use client-side routers and server-side requests/response are "breaking the web".
* You can configure origin request and viewer request/response that goes through CloudFront, this is helpful for example for adding headers.
* Redirecting on Origin Request.

### Test Security

* Mozilla Observatory -> Adding Security Headers
