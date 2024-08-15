# Deploying a Static Website with a Custom Domain on AWS

## Introduction
This document provides a detailed tutorial for deploying a static website on AWS using a custom domain. The steps include setting up an S3 bucket, uploading website files, configuring S3 for static website hosting, setting up Route 53 for domain management, and optionally configuring CloudFront for enhanced performance.

## Prerequisites
- An AWS account
- Basic understanding of web hosting and domain names
- A custom domain name purchased from a domain registrar

## Step 1: Setting Up an S3 Bucket

1. **Log in to AWS Management Console:**
   - Navigate to the [AWS Management Console](https://aws.amazon.com/console/).
   - Sign in with your AWS credentials.

2. **Create an S3 Bucket:**
   - Go to the **S3** service (search for "S3" in the console search bar).
   - Click **Create bucket**.
   - Enter a unique bucket name (e.g., `example-com`), which should match your domain name.
   - Select a region (choose the region closest to your target audience).
   - Leave the other settings as default and click **Create bucket**.

## Step 2: Uploading Website Files to S3

1. **Upload Your Website Files:**
   - Go to your S3 bucket.
   - Click on the **Upload** button.
   - Drag and drop your static website files (HTML, CSS, JavaScript) into the upload window or use the file picker.
   - Click **Upload** to start the upload process.

## Step 3: Configuring S3 for Static Website Hosting

1. **Configure Static Website Hosting:**
   - In your S3 bucket, go to the **Properties** tab.
   - Scroll down to the **Static website hosting** section.
   - Click **Edit**.
   - Select **Enable**.
   - Set the **Index document** to `index.html` (or your homepage file name).
   - Set the **Error document** to `error.html` (if you have a custom error page).
   - Click **Save changes**.

2. **Set Bucket Policy:**
   - Go to the **Permissions** tab of your bucket.
   - Click **Bucket policy**.
   - Add the following policy to make your bucket content publicly accessible:

     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Principal": "*",
           "Action": "s3:GetObject",
           "Resource": "arn:aws:s3:::example-com/*"
         }
       ]
     }
     ```
   - Replace `example-com` with your bucket name.
   - Click **Save changes**.

## Step 4: Setting Up Route 53 for Domain Management

1. **Create a Hosted Zone:**
   - Go to the **Route 53** service.
   - Click **Hosted zones** in the sidebar.
   - Click **Create hosted zone**.
   - Enter your domain name (e.g., `example.com`).
   - Click **Create hosted zone**.

2. **Update Domain Registrar:**
   - Note the **Name servers** provided by Route 53.
   - Go to your domain registrar’s website.
   - Find the DNS settings for your domain.
   - Replace the existing name servers with the ones provided by Route 53.

## Step 5: Configuring CloudFront (Optional but Recommended)

1. **Create a CloudFront Distribution:**
   - Go to the **CloudFront** service.
   - Click **Create Distribution**.
   - Choose **Web** for the delivery method.
   - Under **Origin Settings**, set the **Origin Domain Name** to your S3 bucket URL (found under **Static website hosting** in S3 properties).
   - Set **Viewer Protocol Policy** to **Redirect HTTP to HTTPS** (recommended for security).
   - Leave other settings as default and click **Create Distribution**.

2. **Update Route 53 Records:**
   - Go back to the **Route 53** hosted zone.
   - Create an **A Record** (Alias) pointing to your CloudFront distribution.

## Step 6: Connecting Your Custom Domain to S3

1. **Create an S3 Website Record in Route 53:**
   - In your Route 53 hosted zone, create a new **A Record**.
   - Set the **Record type** to **A – IPv4 address**.
   - For **Alias**, choose **Yes**.
   - For **Alias Target**, select your S3 bucket’s website endpoint or CloudFront distribution (if using CloudFront).
   - Click **Create records**.

2. **Verify Your Domain:**
   - Wait for DNS changes to propagate (this may take some time).
   - Access your custom domain (e.g., `www.example.com`) in a web browser to see your static website.

## Conclusion

Congratulations! You’ve successfully deployed a static website on AWS with a custom domain. You’ve utilised Amazon S3 for scalable hosting, Route 53 for domain management, and optionally CloudFront for enhanced performance and security.

For further questions or troubleshooting, refer to AWS documentation or contact AWS support.
