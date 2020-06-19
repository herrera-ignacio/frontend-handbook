# AWS for Front-End Engineers

## Deploying Single Page Apps

### Key concepts

* Hosting on AWS
* Distribute globally
* Secure with SLL
* Automatic Deployment (CI/CD)
* Dynamic response to request

### Setting up account

* Securint root account
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

* Improve deployment to S3
* `aws configure`
* `.aws/credentials` (you can have multiple)
* `aws s3 ls s3://yourbucket`
* `aws s3 cp my-folder/ s3://mybucket/ --recursive`

### Route 53

* Creating A (Alias) record to route from your domain to your S3 bucket
* Create SSL certificate with AWS Certificate Manager
* Client-side Routing error page
* Setting up a WWW Subdomain, using a www. bucket and redirecting requests

### CloudFront

* CDN for Web, globally distribute application.
* Use your static website domain in S3 as Origin Domain Name for CDN, not S3 bucket url.
* Redirect to HTTPS
* Distribution Settings
	* Add CNAMEs as Alternate Domain Names
	* Custom SSL Certificate
	* Default root object, `/index.html`
* gzipping for our assets
* Change DNS records to CloudFront distribution domain name
* Caching S3 resources, means many fewer requests to S3, reducing costs.


















