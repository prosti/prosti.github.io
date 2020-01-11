---
id: 1639
title: Some jQuery considerations on Thursday
date: 2013-08-01 14:10:27
author: taimane
layout: post
permalink: /some-jquery-considerations-on-thursday/
published: true
categories:
   -
tags:
   -
---
jQuery function to get URL parameter:
<code lang="php">
function getURLParameter(name) {
       return decodeURI(
          (RegExp(name + '=' + '(.+?)(&|$)').exec(location.search)||[,null])[1]
       );
      }
</code>

Here is the jQuery to get document referrer base name:

<code lang="php">
function getReferrerBase() {
url = document.referrer; 
ref = url.match(/:\/\/(.[^/]+)/)[1];
return ref;
}
</code>

Thanks  
