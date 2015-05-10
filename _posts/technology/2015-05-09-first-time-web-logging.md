---
layout: post
category: technology
tagline: Part 1
tags: [jekyll, bootstrap, s3_website, aws]
---
{% include JB/setup %}

## Purpose

Why not.  Having someone else serve your content is not fun, and you can brag about it on your own site at the same time.

## Prerequisites

The first post of this technology blog will go over a tutorial of setting up the prerequisites for hosting static content.  Before you begin, you may want to have the following set up already.

- Github account
- AWS
- Personal DNS domain
- Coins (pennies and dimes)

This also assumes that the markup for your posts are for anyone to download and view.  Github will be used as a platform for managing content updates and new posts.  Git is powerful for working with an offline copy of content.

Subscribing to Amazon Web Services and Github should be relatively straightforward, and getting a DNS name should be trivial as well.  Most blogs are hosted entirely by 3rd parties, whereas this approach allows you to easily transfer from one hosting provider to another.  I can easily host this on an apache server, my favorite CDN, or another cloud storage provider.

## Set up Jekyll

Set up a t2.micro EC2 instance which will serve as a deployment server.  In my case, I went with the Amazon Linux AMI.

On the instance, install all the necessary dependencies to run Jekyll, s3_website, and any other tools to construct the website.  Nodejs appears to be the only package that needs the EPEL repository enabled.

    yum install rubygem-io-console ruby-devel gcc git rake
    yum --enablerepo=epel install nodejs
    gem install jekyll
    gem install s3_website

Then, clone the jekyll bootstrap github project.

    git clone https://github.com/plusjade/jekyll-bootstrap.git

Next set up s3_website.yml within jekyll-bootstrap.  The following configuration works well.  Note that the _site directory is where Jekyll builds its output.

    s3_id: <%= ENV['S3_ID'] %>
    s3_secret: <%= ENV['S3_SECRET'] %>
    s3_bucket: <%= ENV['S3_BUCKET'] %>
    
    site: _site
    
    max_age:
       "assets/*": 6000
       "*": 300
    
    gzip:
       - .html
       - .css
       - .md 
    
    s3_reduced_redundancy: true

Then run `jekyll build` to build the sample website.

## Set up s3_website to AWS

Create a .env file with 600 permissions in the same directory.  The contents of the file should contain the following.  As of writing, s3_website did not use the instance profile credentials found on EC2 instances, which would avoid having credentials stored in clear text.  However, this instance will not need to run in order for the site to function as the site is entirely static.

    S3_ID=<AWS Access Key>
    S3_SECRET=<AWS Secret Key>
    S3_BUCKET=<website domain>

You will want to dedicate an IAM user just for uploading S3 content.  I created a site-admin IAM user for this purpose.  The IAM policy for this user should look like the following:

    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "Stmt1430956885000",
                "Effect": "Deny",
                "Action": [
                    "s3:DeleteBucket",
                    "s3:DeleteBucketPolicy",
                    "s3:DeleteBucketWebsite"
                ],
                "Resource": [
                    "*"
                ]
            },
            {
                "Action": [
                    "cloudfront:*"
                ],
                "Effect": "Allow",
                "Resource": [
                    "*"
                ]
            },
            {
                "Sid": "Stmt1430956830000",
                "Effect": "Allow",
                "Action": [
                    "s3:*"
                ],
                "Resource": [
                    "arn:aws:s3:::<website name>",
                    "arn:aws:s3:::<website name>/*"
                ]
            }
        ]
    }

s3_website configures your S3 buckets as static sites for you.  Run s3_website to have it create the appropriate buckets while uploading the contents of the Jekyll build.

## Set up Route 53

First you will want to create a hosted zone for your domain.  Within this hosted zone, you will want to create an ALIAS record to the bucket s3_website created.  Once this is created, you should be able to visit your site and you should see the sample Jekyll page.

## Write content

You should be able to create files under _posts.  The core-samples directory demonstrates the markup language quite well.

## Next steps

Some improvements can be made to the current version of Jekyll Bootstrap.  Some of the static files could be replaced by CDN hosted copies of Bootstrap and JQuery.  In addition, given that bootstrap is used, you should be able to easily swap out themes to your liking.  Each of the bootswatch themes appear to work well with some modifications to the _includes files.

The following changes were made to Jekyll Bootstrap in order to apply the changes I needed (addition of a site license, as well as the ability to select different [bootstrap themes](https://github.com/atokhy/jekyll-bootstrap/commit/a7f49fb1d8a00b46ebf80a9b87660f30b72d0c37).

