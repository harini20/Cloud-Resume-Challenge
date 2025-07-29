## Cloud-Resume-Challenge
🛠️ AWS SERVICES USED:
- AWS S3
- AWS CloudFront
- AWS Lambda - Python
- AWS Dynamo DB


✅ Troubleshooting:

During the deployment of my Cloud Resume Challenge, I encountered and resolved several issues that strengthened my understanding of AWS services and web hosting best practices:

1. ❌ “404 Not Found – NoSuchWebsiteConfiguration”
   
Issue:
When I first accessed my S3 bucket’s endpoint, I received a 404 error indicating that the website configuration was missing.

Root Cause:
Although the index.html file was uploaded, static website hosting had not been enabled for the S3 bucket.

Fix:
I enabled static website hosting in the S3 bucket settings and specified index.html as the index document. This allowed the content to be served properly from the S3 endpoint.

2. ❌ “403 Forbidden – Access Denied via CloudFront”
   
Issue:
After integrating CloudFront, attempting to load the site through the distribution URL returned a 403 error.

Root Cause:
The S3 bucket was private, and CloudFront didn’t have permission to fetch the content.

Fix:
I switched from the S3 website endpoint (s3-website-...) to the S3 REST API endpoint (s3.<region>.amazonaws.com), which supports Origin Access Control (OAC). I then created an OAC and attached it to CloudFront. Finally, I updated the bucket policy to allow s3:GetObject access from my specific CloudFront distribution using the SourceArn condition.

✅ Outcome

All issues were successfully resolved, and the resume site is now:

- Fully functional and styled

- Delivered securely via CloudFront with private S3 bucket access

- Efficiently cached and updatable with invalidation when needed

This debugging journey helped reinforce core AWS concepts such as IAM policies, S3 static hosting, CloudFront OAC, and cache control.

3. ❌ “500 Internal Server Error – Lambda Function URL Returning Invalid Response”

Issue: Internal Server Error with AWS Lambda Function URL
While integrating AWS Lambda with DynamoDB for the Cloud Resume Challenge, I encountered an HTTP 500 Internal Server Error when accessing the Lambda Function URL.
Instead of returning the views count, the browser showed “500 Internal Server Error”.

The error I received after printing the error code to debug:

{"error": "An error occurred (AccessDeniedException) when calling the GetItem operation: User: arn:aws:sts::433607260416:assumed-role/cloud-resume-test-api-role-f6ugnhmr/cloud-resume-test-api is not authorized to perform: dynamodb:GetItem on resource: arn:aws:dynamodb:eu-west-1:433607260416:table/cloud-resume-test because no identity-based policy allows the dynamodb:GetItem action"}

Error: My Lambda function does not have permission to read/write my DynamoDB table.

Cause: Lambda execution role didn’t have permission to call dynamodb:GetItem or PutItem.

Fix: Attached AmazonDynamoDBFullAccess to the Lambda execution role, which resolved the error and allowed the function to fetch and update the views count successfully.

4. ❌ “CORS Error – Lambda Function URL Fails to Return Viewer Count”
Issue:
While integrating the Lambda Function URL with my frontend for the viewer counter, I encountered a persistent CORS error. The frontend JavaScript fetch call fails, and the viewer count doesn’t display.

Instead of receiving the expected JSON response from Lambda, the browser blocks the request with a CORS policy error.

Error in browser:

Access to fetch at 'https://6xlttcehhwiug5pseu6vfcbayqvlvvs.lambda-url.eu-west-1.on.aws/' 

from origin 'null' has been blocked by CORS policy: 

The 'Access-Control-Allow-Origin' header contains multiple values '*', '*'. 

Only one is allowed.

❓ Fix Attempted:

Went to Lambda > Configuration > Function URL > Edit CORS

Set:

Allowed origins: *

Allowed methods: GET

Allowed headers: *

Credentials: set to false

Also validated that frontend fetch() is using the correct URL and parses data.views

