## Cloud-Resume-Challenge
🛠️ AWS SERVICES USED:
- AWS S3
- AWS CloudFront


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
