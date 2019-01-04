---
id: 860
title: Talk to the BOT
date: 2009-05-28T19:02:57+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=860
permalink: /talk-to-the-bot/
jd_tweet_this:
  - 'yes'
jd_twitter:
  - '[New Blog Post] Talk to the BOT. Interactive IM BOTS can improve government service delivery. #url#'
wp_jd_clig:
  - http://cli.gs/Jn9svJ
original_post_id:
  - "860"
categories:
  - Development Tools
  - Tutorials
---
With governments under enormous budgetary pressure, finding ways to provide service at lower cost is more important than ever. The term &#8220;&#8216;automated self-service&#8221; usually conjures up thoughts of web applications or interactive voice response (IVR) systems, but there are other ways that governments (and other organizations) can enable citizens and customers to self serve.

One intriguing new way to enable self service is to deploy instant messaging (IM) applications, sometimes referred to as IM &#8220;BOTS&#8221;. Personally, I would love to be able to make a park reservation or find out the wait time at the DMV by simply opening up an IM client &#8211; perhaps one on my iPhone &#8211; and entering a few simple commands.

Writing an IM BOT might usually require some in-depth knowledge of XMPP or specific IM platform requirements, but a simple and effective way of writing an IM BOT that can work with a number of different IM services is to use the <a href="http://www.imified.com/" target="_blank">IMified Platform</a>.

IMified, which was recently <a href="http://blog.imified.com/index.php/2009/05/27/voxeo-acquires-imified/" target="_blank">bought by Voxeo</a>, provides a simple and effective API for developing interactive IM BOTS. Essentially, IMified provides a IM front end to a web application that can be used to interact with back end databases, web services, etc. Communication between your web application and the IMified platform is in HTTP, so any web programming language can be used to write an IM BOT. (This architectural set up is very similar to the services provided by <a href="http://evolution.voxeo.com/" target="_blank">Voxeo&#8217;s hosted offering</a> &#8211; they provide a front end to the <acronym title="Public Switched Telephone Network">PSTN</acronym> and allow you to write telephone applications in a variety of traditional web programming languages.)

When a user interacts with your IM BOT, the IMified platform will make an HTTP request (using the POST method) to your web application and allow you to write out information in the response. A quick read of their <a href="http://www.imified.com/developers/api" target="_blank">API documentation</a> will get you started on your first BOT. There is also a very nice debugger tool provided by IMified that allows developers to interact with their application and get access to the name/value pairs sent in each successive HTTP request &#8211; a very handy tool indeed!

Having said all of that, you might be trying to reconcile the notion of using HTTP with an interactive BOT. Self service usually requiers some sort of state management, and HTTP is stateless. IMified provides a nifty mechanism for managing state during your interaction with a user of your BOT. Among the name/value pairs sent to your web application with each request from the IMified platform are the following:

> **userkey**: a unique token generated by IMified when presence is established with an IM user sending a message to your bot. This userkey uniquely identifies a user of your bot and can be used for instance, to associate the IM user with a user in your system.
> 
> **value1 &#8211; valuex** are the values entered by the IM user for each respective step in the interaction. So a user that has entered text multiple times might generate a request with &#8211; value0=hello, value1=My name is Joe, valueX=Nice BOT you have there, etc. 

So, as your user (identifiable via _userkey_) interacts with your BOT, the list of values is increased, with a new name/value pair being added for each turn. This makes it easy to collect information, and even to verify new input based on previous responses.

After playing around with IMified, I decided that I wanted to write a sample app that used a small PHP class to make managing the information provided in each HTTP request more manageable. The code for the IMified class is as follows:

A simple example BOT demonstrates the power of this class, and how easy it is to create your own IM BOT on the IMified platform:

<pre>getStep();

  echo "Hello. Your IM network is: ". $testBotMessage-&gt;getNetwork().".<br />";
  echo "This is step ".$step.".<br />";
  if($step &gt; 1)
    echo "In the last step, you said ".$testBotMessage-&gt;getLastValue().".";

}

catch (Exception $ex) {
  echo "Sorry. The Bot is having some issues: ".$ex-&gt;getMessage();
}

?&gt;
</pre>

In the above example, we create a new instance of the IMified class and pass the <a href="http://us.php.net/manual/en/reserved.variables.post.php" target="_blank">$_POST superglobal array</a> into its constructor. This will automatically assign all of the data in the HTTP request to the object&#8217;s properties and, as in the case of the _step_ value, cast to the proper type:

<pre>...
$this-&gt;step = (int) $postValues['step'];
...
</pre>

What I really like about this class is that it takes any user interaction values submitted with the request and packages them up neatly into an array &#8211; this array is assigned to one of the class properties, and there are several accessor methods that let you work with responses submitted by the BOT user. You can see this demo BOT in action by adding **voiceingovtest@bot.im** to your Jabber/XMPP client contacts and sending it a message (note &#8211; you can use the Google Talk client for this).

Governments interested in creating new service delivery channels that can lower costs and increase customer satisfaction should take a close look at IM BOTS in general, and the powerful combination of Voxeo/IMified in particular.

I hope to expand on the simple example shown here and further build out the IMified class to demonstrate the power of interactive IM BOTS to improve government service delivery.

Stay tuned!