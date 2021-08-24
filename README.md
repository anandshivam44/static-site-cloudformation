## CloudFormation Template for a static website on a private s3 bucket and attached to CloudFront
 - [x] Static site hosted on s3 bucket
 - [x] Bucket is private
 - [x] Global deployment and cache using CloudFront

#### Pre requisites
1. AWS Account
2. You need to have AWS CLI configured if you are using AWS CLI (optional)
##### Getting started
```
git clone https://github.com/anandshivam44/secure-static-site.git
cd secure-static-site
```
##### Save Env Variables
```bash
export STACK_NAME=<STACK NAME>
export BUCKET_NAME=<BUCKET NAME>
export REGION=<REGION NAME>
```
Replace `<STACK NAME>`, `<BUCKET NAME>` and `<REGION NAME>` with a stack name of your choice, your own bucket name that you want to create and a region name where you want to deploy 

##### Deploy
Deploy the template using AWS CF Console or use AWS CLI 

```
aws cloudformation create-stack \
--stack-name $STACK_NAME \
--template-body file://static-site-cloudfront.yaml \
--parameters ParameterKey=S3StaticSiteBucketName,ParameterValue=$BUCKET_NAME \
ParameterKey=Region,ParameterValue=$REGION
```
##### Copy your website files

Once the Template is deployed on AWS CF
Copy/Create/Upload your html files to the bucket
Here is one for quick deployment.  
  

###### index.html
```html
<head>
    <title>My Website Home Page</title>
</head>
<body>
  <h1>Welcome to my website</h1>
  <p>Now hosted on Amazon S3!</p>
</body>
</html>
```
or I've added some Samples in `StaticSiteExample1` and `StaticSiteExample2`

Copy file/folder to your bucket
```bash
aws s3 cp index.html s3://$BUCKET_NAME
```
  

##### Access your site
Goto CloudFront and wait until the deployment is ready. It will take some time.
Once Ready access your site with the url provided by CloudFront

##### Cleaning up
Empty the bucket before removing CF Stack
```bash
aws s3 rm s3://$BUCKET_NAME/* --recursive
```
Delete CloudFormationt Stack
```bash
aws cloudformation delete-stack --stack-name $STACK_NAME
```
