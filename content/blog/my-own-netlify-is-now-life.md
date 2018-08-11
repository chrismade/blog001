---
layout: blog
title: my own netlify is now life
date: '2018-08-11T08:51:31+02:00'
thumbnail: /images/cfv4b.jpg
categories:
  - news
---
following the instructions on <https://www.control-alt-del.org/post/serverless-blog-howto/> and fixing [an issue in the config.yml](https://github.com/marksteele/netlify-serverless-oauth2-backend/issues/3) the serverless blog is now life.

## The CMS back-End

The CMS backend with workflow and edit functions reside in three files:

A single javascript file which incorporates every functionality which is needed

A single configuration file config.yml which keeps all config data that is needed by the javascript CMS

A single HTML which invokes the javascript file and which could also add custom widgets to extend the base functionality, e.g. a picker which let you find and include YouTube files into your blog without copying the HTML code for embedding

That's all - all execution happens in the browser - there is a little pushback as of this writing (Aug 2018) - Firefox LTS which is widely used in enterprise installations throws [an error message which makes page editing unusable](https://github.com/netlify/netlify-cms/issues/959) - and netlify decided not to fix this. Obviously they don't have much enterprise customers at the moment. But the software is open-source, so you can fork the CMS, fix it yourself and issue a PR.

## The repository

As persistence layer github is used as repository - which makes it pretty easy to have versioning while not caring on scalability

## The page generator

HUGO is used as page generator - offering a wide-range of freely available templates which create static sites - travisCI is called every time a change is issued in the repository master branch (publish) and travisCI invokes hugo to generate a new site - which is pretty fast

## The target desitination

In order to see the site live and still follow the paradigm of a serverless architecture github pages is used - so the source files are read from the github repository and the outcome of the build process is - again - stored in github in another place. Github pages take care of a valid SSL certificate and that the site appear as a regular website which you can see in your browser -  

## The authentication

When
