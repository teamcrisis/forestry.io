---
images:
- "/uploads/2018/01/OGimage-01-docs-3x.jpg"
publishdate: 2019-01-08 17:00:00 -1100
authors:
- Sebastian Engels
expirydate: 2030-01-01 04:00:00 +0000
layout: single
title: Cloud Media Storage with AWS S3
categories:
- CMS
headline: ''
description: ''
textline: ''
tags: []
cta:
  headline: ''
  textline: ''
  calls_to_action: []
private: false
weight: ''
aliases: []
date: 2019-01-08 02:34:02 -1100
draft: true

---
Forestry supports storing your media in your own [AWS S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html).

## Linking Your S3 Account

![](/uploads/2019/01/s3-settings.png)

1. [Sign up for an AWS account](https://portal.aws.amazon.com/billing/signup#/start)
2. Log in to your dashboard and click open the _Services_ dropdown in the top bar then click on the S3 service.
3. Create a Bucket and make not of the Bucket name for later. In the _Set Permissions_ step ensure to uncheck the following options:
   * Block new public ACLs and uploading public objects
   * Remove public access granted through public ACLs

   {{% tip %}}

   If this is your first time creating a bucket simply keep the default settings if not noted otherwise.

   {{% /tip %}}
4. Open up your site's dashboard in Forestry and navigate to the **Settings** section, and select the **Media** tab.
5. Under **Media Management**, select **Cloudinary**. You should see fields appear for **Cloud name**, **API key**, and **API secret**.
6. Enter your Cloudinary info int these fields and click the **Save Settings** button.
7. Navigate to the **Media** section in the Forestry dashboard. If you see the Cloudinary sample image in your library, you are successfully connected!

![Cloudinary sample image in media library](/uploads/2018/05/media-library-cloudinary.png)

## Recommended Path Settings

You will want to adjust your paths in the **Media** section to work with Cloudinary.

* **Public Path**: You will want to enter the full URL to your Cloudinary cloud. This will look something like `[https://res.cloudinary.com/](https://res.cloudinary.com/ "https://res.cloudinary.com/")``{{ CLOUD_NAME }}/image/upload/`.
* **Front Matter Path:** Activate the toggle in this section to use a different **public path** when inserting an image into front matter, and leave the value empty. This will allow you to easily insert URL transformations ([see below](#using-cloudinary-images)).

### Applying transforms to markdown content

Your markdown content will not have any transforms applied by default. To apply transforms to all of your markdown images, you can use the **public path** and **front matter path** separately.

E.g:

* **Public Path:** `[https://res.cloudinary.com/](https://res.cloudinary.com/ "https://res.cloudinary.com/")``{{ CLOUD_NAME }}/image/upload/c_scale,w_auto:100,dpr_auto/`
* **Front Matter Path:** `[https://res.cloudinary.com/](https://res.cloudinary.com/ "https://res.cloudinary.com/")``{{ CLOUD_NAME }}/image/upload/`

## Migrating Existing Media

Currently, we are not able to automatically migrate existing media from a git repo to a user's Cloudinary account. We recommend logging in to your Cloudinary dashboard and bulk uploading your images there. After the images are in your Cloudinary account, they will also appear in your Media Library in Forestry.

Unfortunately, for the time being, you will need to manually update your front matter to reassign the new images.

## Using Cloudinary Images

Cloudinary supports a vast array of image manipulation options that can be implemented directly in the image URL. Cloudinary has [so many transformation options](https://cloudinary.com/documentation/image_transformations) that, rather than attempt to support a subset of them in our UI, we have instead decided to make it easy for developers to configure these image transformations themselves in the site's templates.

When adding a Cloudinary image to a front matter field, Forestry will only save the base filename in the page's front matter, like this:

    ---
    main_image: img.png
    #...
    ---

When using these images in your templates, you should prepend them with the URL to your cloud. We recommend storing this URL in your site's configuration and referencing it via variable in your templates. For example, in a Hugo site you might put the following in `config.toml`:

    cloudinary_url = "https://res.cloudinary.com/demo/image/upload/"

You could then use these front matter images in your templates as follows:

    <img src="{{ .Site.cloudinary_url }}/{{ .Params.main_image }}" />

Using this approach, adding in one or more transformations is easy and transparent:

    <img src="{{ .Site.cloudinary_url }}/w_250,h_250,c_fill/{{ .Params.main_image }}">

{{% tip %}} View Cloudinary's [image transformations documentation](https://cloudinary.com/documentation/image_transformations) to check out the wide array of things you can do with Cloudinary! {{% /tip %}}