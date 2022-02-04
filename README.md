## AWS CloudFormation Template for a static website on a private AWS s3 bucket and hosted on AWS CloudFront
### IaaC Examlpe
 - [x] Static site hosted on s3 bucket
 - [x] Bucket is private, no one can DDoS and abuse your bucket. Only CloudFront can access your bucket.
 - [x] Global deployment and cache using AWS CloudFront

#### Pre requisites
1. AWS Account
2. You need to have AWS CLI configured
##### Getting started
```
git clone https://github.com/anandshivam44/secure-static-site.git
cd secure-static-site
```
##### Save Env Variables
```bash
export STACK_NAME=MyStaticSiteStack
export STATIC_SITE_BUCKET_NAME=<A unique Bucket Name>
export LOGGING_BUCKET_NAME=<A unique Bucket Name>
export REGION=us-east-1

```
You can use a StackName and Bucket of your choice, Bucket name should be unique.  
You can choose among AWS Regions of your choice

##### Deploy
Deploy the template using AWS CF Console or use AWS CLI 

```
aws cloudformation create-stack \
--stack-name $STACK_NAME \
--template-body file://static-site-cloudfront.yaml \
--parameters ParameterKey=S3StaticSiteBucketName,ParameterValue=$BUCKET_NAME \
ParameterKey=Region,ParameterValue=$REGION \
ParameterKey=S3LoggingBucketName,ParameterValue=$LOGGING_BUCKET_NAME
```
##### Copy your website files

Once the Template is deployed on AWS CF which takes around 30 sec - 1 minute
Copy/Create/Upload your html files to the bucket  

This Github repo has 2 examples `StaticSiteExample1` and `StaticSiteExample2`. Both are individual websites. For the sake of demo we will be using `StaticSiteExample1`

Copy file(s) to your bucket
```bash
aws s3 cp ./StaticSiteExample1/ s3://$STATIC_SITE_BUCKET_NAME --recursive
```
Verify `index.html` is at the root of the bucket
```bash
aws s3 ls s3://$STATIC_SITE_BUCKET_NAME
```
If you see index.html file. You are good to go. 
  

##### Access your site
To get the url
```bash
aws cloudformation describe-stacks --stack-name my-static-site-stack | grep Output
```
Check the output for your website url, which looks like `xxxxxxx.cloudfront.net`
Copy paste this url in your website.
**Hurray** your website is up and running.




##### Cleaning up
To avoid unexpected changes make sure you clean up properly and leave no trace.
Empty the bucket before removing CF Stack
```bash
aws s3 rm s3://$STATIC_SITE_BUCKET_NAME/ --recursive
```
Delete CloudFormationt Stack
```bash
aws cloudformation delete-stack --stack-name $STACK_NAME
```
