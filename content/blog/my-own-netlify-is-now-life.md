---
layout: blog
title: my serverless blog is now life
date: '2018-08-11T08:51:31+02:00'
thumbnail: /images/cfv4b.jpg
categories:
  - news
---
following the instructions on <https://www.control-alt-del.org/post/serverless-blog-howto/> and fixing [an issue in the config.yml](https://github.com/marksteele/netlify-serverless-oauth2-backend/issues/3) the serverless blog is now life.

## The CMS Back-End

The CMS backend with workflow and edit functions resides in three files:

1. A single javascript file which incorporates every functionality which is needed
2. A single configuration file config.yml which keeps all config data that is needed by the javascript CMS
3. A single HTML which invokes the javascript file and which could also add custom widgets to extend the base functionality, e.g. a picker which let you find and include YouTube files into your blog without copying the HTML code for embedding

That's all - all execution happens in the browser - there is a little pushback as of this writing (Dec 2017 till Aug 2018) - Firefox LTS - which is widely used in enterprise client installations - throws [an error message which makes page editing unusable](https://github.com/netlify/netlify-cms/issues/959) - and netlify decided not to fix this. Obviously they don't have much enterprise customers at the moment. But the software is open-source, so you can [fork the CMS](https://github.com/netlify/netlify-cms), fix it yourself and issue a PR.

## The repository

As persistence layer github is used as repository - which makes it pretty easy to have versioning while not worrying on scalability - which is pretty often a pain in traditional CMS solution when the number of pages is growing and lot of editing creates lots of versions. github - as we all know - was never designed as a CMS for building wesites but it is interesting to see that every use-case in source-control of software development has an equivalent in web content management - and vice versa - and how both approaches converge - for a closer look you might be interested in [this series of blog articles](https://www.thoughtworks.com/insights/blog/incremental-approach-content-management-using-git) - which resulted in a proof-of-concept called [hacienda.io](http://hacienda.io/) - which unfortunately haven't seen much (visible) development in the past three years - but the underlying concept and thoughts are still valid.

## The page generator

[HUGO](https://gohugo.io/) is used as page generator - offering a wide-range of [freely available themes](https://themes.gohugo.io/) which create static sites - travisCI is called every time a change is issued in the repository master branch (publish) and travisCI invokes hugo to generate a new site - which is pretty fast

## The target destination

In order to see the site live and still follow the paradigm of a serverless architecture github pages is used - so the source files are read from the github repository and the outcome of the build process is - again - stored in github in another place. Github pages takes care of a valid SSL certificate and that the site appear as a regular website which you can see in your browser -  

If you don't like the given URL (precisely FQDN) from github pages then you can [follow these instructions](https://help.github.com/articles/using-a-custom-domain-with-github-pages/) to make it appear in your own custom domain - of course in this case you need to take care of the SSL certificate yourself and you may want to consider using [letsencrypt](https://letsencrypt.org/) if you don't have a better option available.

You may think a static website is a concept of the past - and you are right - but like in fashion some trends return - and especially if most aspects on the downside of a concept as vanished - a static website in 2018 - unlike to the ones in 1998 - may appear pretty dynamic and interactive thanks to frameworks like react, angular, vueJS or [EmberJS](https://www.emberjs.com/) - while the upsite is still true e.g. fast in delivery and almost invulnerable to most kinds of attacs. Read [this article](https://medium.com/netlify/webserverless-32f117e35996) for a detailed discussion.

## The authentication

When everything is so open - javascript, public github repositories for source and target - where do you put secrets which must not be visible to anybody else? The OAUTH authentication protocol requires a server to issue the request and handles the callback - so this can be achieved by using AWS lambda - or similar functions which exists in Microsoft Azure Functions or Google CloudFunctions - if you are unfamiliar with writing such functions yourself then check out <https://github.com/serverless/serverless> - this is an universal framework which helps you writing, debugging and uploading such functions in a vendor-agnostic way - with a full bunch of plug-ins for lots of use-cases - for this CMS authentification Mark Steele used this framework to handle the OAUTH protocol allowing login via GitHub and to store its secrets in AWS KMS.

## From here

There is a ready-made service you can subscribe for free on netlify.com - which is pretty convenient if you are not so interested in the technical details and just want to start quickly building a site - but putting the pieces together step-by-step yourself reveals the beauty of this architecture and get hands-on experience on state-of-the-art approaches for traditional use-cases - the use of github as source and target repository and authentication provider is a shortcut - as you are doing all operations in this single github identity - have a look into the netlify documentation in order to discover more options, e.g. use different authentication providers or manage identities yourself based on "netlify identity" (which is based on GoTrue API) and using a github gateway in case your authentication provider is different from github.

It is also worth mentioning that netlify is not tied to HUGO for building your static website - for example you can also use [gatsby](https://www.gatsbyjs.org/) if you prefer - they simply want to keep their architecture open to cope with any new development which may come tomorrow.
