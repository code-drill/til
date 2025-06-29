<!--
.. title: Simple Domain Redirect On AWS
.. slug: simple-domain-redirect-on-aws
.. date: 2025-06-29 21:58:07 UTC+02:00
.. tags: aws, route 53, s3, 
.. category: aws
.. link: 
.. description: 
.. type: text
-->

Assuming you have some domain configured on AWS using Route 53: [example.eu](https://example.eu)  
And now you want to redirect all traffic from [example.eu](https://example.eu) to [blog.example.eu](https://blog.example.eu)

### Step 1: Create S3 Bucket

1. S3 Console -> Create bucket
2. Bucket name: `example.eu` (must match your domain exactly)
3. Keep all default settings -> Create bucket

### Step 2: Configure S3 for Redirect

1. Select your bucket -> Properties tab
2. Static website hosting -> Edit
3. Enable static website hosting
4. Hosting type: Redirect requests for an object
5. Host name: `blog.example.eu`
6. Protocol: `https`
7. Save changes

Copy the S3 website endpoint (e.g., `http://example.eu.s3-website-us-east-1.amazonaws.com`)

### Step 3: Request SSL Certificate

Important: Must be done in us-east-1 region

1. Certificate Manager -> Request certificate
2. Domain name: `example.eu`
3. Validation: DNS validation
4. Request certificate
5. Create records in Route 53 (button will appear)
6. Wait for "Issued" status (5-10 minutes)

### Step 4: Create CloudFront Distribution

1. CloudFront Console -> Create distribution
2. Origin domain: Paste your S3 website endpoint from Step 2 (e.g., `http://example.eu.s3-website-us-east-1.amazonaws.com`)
3. Protocol: HTTP only
4. Viewer protocol policy: Redirect HTTP to HTTPS
5. Alternate domain names: `example.eu`
6. SSL certificate: Select your certificate from Step 3
7. Create distribution

Wait 15-20 minutes for deployment

### Step 5: Update Route 53

1. Route 53 -> Hosted zones -> example.eu
2. Create record
3. Record name: Leave empty
4. Record type: A
5. Alias: Yes
6. Route traffic to: CloudFront distribution
7. Select your distribution
8. Create records

Wait 5~20 minutes, then test:  
- Opening `http://example.eu` -> should redirect to `https://blog.example.eu`  
- Opening `https://example.eu` -> should redirect to `https://blog.example.eu`
