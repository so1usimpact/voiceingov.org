---
id: 1625
title: 'Cloudvox: Cloud Telephony Applications Made Easy'
date: 2010-02-25T11:20:13+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=1625
permalink: /cloudvox-cloud-telephony-applications-made-easy/
jd_tweet_this:
  - 'yes'
jd_twitter:
  - '[New Blog Post] Cloud Telephony Applications Made Easy With #Cloudvox. #url# #ivr #voip #php #ifbyphone'
wp_jd_clig:
  - http://cli.gs/0stLy
wp_jd_target:
  - ""
original_post_id:
  - "1625"
categories:
  - Development Tools
  - Open Source
  - Tutorials
---
A couple of months ago, I did [a quick write up](http://www.voiceingov.org/blog/?p=1235) on a new cloud telephony company named [Cloudvox](http://www.cloudvox.com/). In the interim, I&#8217;ve been doing some playing around with their [HTTP/JSON API for creating telephone applications](http://help.cloudvox.com/faqs/getting-started/http-api), and I&#8217;ve been blown away by how simple and powerful a tool it is for building sophisticated cloud telephony applications.

This post will provide a quick overview of how the Cloudvox JSON API can be paired with PHP and the delightfully awesome [Limonade](http://limonade.sofa-design.net/) Framework. If you&#8217;re not a PHP developer don&#8217;t despair &#8211; this example can easily be ported to other languages like Ruby (using [Sinatra](http://www.sinatrarb.com/)) or C# (using [Kayak](http://kayakhttp.com/)).

### Cloudvox API Helper Classes

When writing apps for the Cloudvox JSON API, there are two things that we need to manage &#8211; the JSON that we will send to Cloudvox (using plain old HTTP) and the response Cloudvox sends back to our app (this will include any user input collected from the caller, and also things like a unique identifier for the call, caller ID and other information about the call).

To make managing both sides of our exchange with Cloudvox easier, I&#8217;ve created a set of PHP classes that can be used with any standard PHP IDE to make writing Cloudvox JSON and parsing Cloudvox responses simple and easy. You can download this class library from [GitHub](http://gist.github.com/308081).

Using these classes is pretty straightforward:

> `</p>
<p>`
> 
> Will render:
> 
> `[{"name":"Speak","phrase":"Hello world!"},{"name":"Hangup"}]`

Required properties are included in the constructor for each class &#8211; in most IDE&#8217;s this means that you can simply use `Shift + Control + Space Bar` when you create a new instance of the object to see what properties are required.

Optional properties on all classes are handled by using the [PHP __set() method](http://www.php.net/manual/en/language.oop5.overloading.php) in the base class. This effectively let&#8217;s you overload the object and set properties which are not declared in the class definition. So for example, if we wanted to collect input from the caller (e.g., their zip code) we would use the GetDigits class, and overload it to add a URL to post the results to:

> `<br />
url = "http://somehost.com/myscript.php";</p>
<p>?><br />
` 

The problem with overloading in PHP is that you don&#8217;t get the benefit of having your IDE display overload options (it can&#8217;t because the properties that we wish to set are not declared in the class definition). There also isn&#8217;t any way to control what overloaded properties get set, so its possible to add things the Cloudvox won&#8217;t understand.

I&#8217;m not sure there is any way around this given the way that PHP has implemented overloading. I do plan to work on a set of Cloudvox classes using another language that handles overloading a bit better, like C#, but for now you should only overload to set the `<strong>url</strong>` and `<strong>method</strong>` properites for classes that can use them (see the Cloudvox docs for more details).

### Sipping on some Limonade

If you know of the excellent Ruby Framework [Sinatra](http://www.sinatrarb.com/) but you want to code your project in PHP, fear not &#8211; check out the equally excellent PHP framework called [Limonade](http://limonade.sofa-design.net/). It&#8217;s the functional equivalent of Sinatra for PHP.

Using Limonade with our set of Cloudvox JSON classes makes building cloud telephony applications very simple. The biggest benefit is that you don&#8217;t have to split up the different steps in your application (i.e., collecting input, validating input, re prompting, etc.) into different PHP files (which can be kind of a pain) &#8211; all of your steps can be contained within a single file.

Limonade lets you set a URL &#8220;route&#8221; that Cloudvox can send HTTP requests to with results, and to get rendered JSON for another application step. For example:

> `</p>
<p>`

This &#8220;Hello World&#8221; example &#8211; defined in a script named `sample.php` &#8211; could be accessed by hitting `http://somehost.com/index.php?/start`. To make things easier, we&#8217;ll use a little Apache magic to allow URL rewriting:

This will let us access the above URL by hitting `http://somehost.com/index.php/start`.

A simple demo app using the Cloudvox JSON helper classes and the Limonade Framework can be seen below. You can use this sample application with a <a href="http://www.cloudvox.com/signup" target="_blank">new Cloudvox account</a> to get started building cloud telephony applications.

This simple app demonstrates how powerful the [Cloudvox JSON API](http://help.cloudvox.com/faqs/getting-started/http-api) is for creating cloud telephony apps. When coupled with an elegant framework like Limonade, sophisticated, cloud-based telephony applications are readily available to any developer that wants to build one.

The helper classes for the Cloudvox API are obviously a work in progress, so if anyone reading this has comments or suggestions feel free to let me know &#8211; mheadd [at] voiceingov.org.