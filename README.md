## Creating an AWS S3 bucket using CloudFormation

<!-- 
AWS S3 buckets can be configured to replicate all objects put in them to another bucket in a different region.
This is called ```Cross Region Replication```.

Why this is useful is that objects stored in a bucket are kept only in the region that they were created in.
Other regions may be able to access them if allowed but if a regional outage were to occur the contents of the buckets in that region may not be accessible.

Note that because S3 buckets have a global namespace it is not possible to have a bucket with the same name in 2 different regions.

Because of this it is useful to name a bucket with a suffix of the region that the bucket was created in.
Doing that allows you to have uniquely named buckets that differ in name by only the region making the functions accessing the contents easier to write and manage.
 -->
This is an example of using CloudFormation to create bucket to store objects in, a bucket to for the domain and a bucket for logging. It will aslo create the DNS records in Route53 for the website and set up logging.

S3StaticWebsiteCustomDomain.yml contains all the code to create the CloudFormation stacks. The sections are listed below. 
 * Parameters - defines root domain
 * Mappings - set in the script to us-east-1
 * Resources
    * SubatomictheoryRootBucket - root bucket containing all the website files
    * SubatomictheoryWWWBucket - configures the website
    * SubatomictheoryLogBucket - logging bucket
    * SubatomictheoryBucketPolicy - policy to allow public read of bucket objects
    * SubatomictheoryDNS - configures Route53 records
 * Outputs - prints out website URLs

To get started run package and deploy the template.
```
$ aws cloudformation package
$ aws cloudformation deploy --template-file S3StaticWebsiteCustomDomain.yml --stack-name SubatomictheoryStack
```

To delete the stack and start fresh
```
$ aws cloudformation delete-stack --stack-name SubatomictheoryStack
```

To sync S3 folder
```
aws s3 sync --acl public-read \
    --exclude '*' \
    --include '*.html' \
    --include '*.png' \
    --include '*.css' \
    "/Users/xyz/AWS/S3BucketSite/" s3://subatomictheory.com
```
