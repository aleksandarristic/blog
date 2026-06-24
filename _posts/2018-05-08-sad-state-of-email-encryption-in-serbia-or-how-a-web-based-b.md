---
title: "Sad state of email encryption in Serbia - or how a web based bank just does not encrypt emails"
date: 2018-05-08 12:32:00 +0000
layout: post
tags:
  - "security"
  - "bank"
  - "TLS"
  - "email"
---

Not long ago, browsing through my received emails in my gmail, I've realized that one of the banks I use does not use TLS when sending emails to their clients. Of course, being a security aware customer of the bank, and having in mind that I am their client, I've decided to contact them with this information, pointing out the fact that Gmail, a 21st century go-to free email provider, makes this very prominent in each mail they send.\
\
The usual ways of contacting the bank in question is either via phone, email or Twitter. Since they don't encrypt their email traffic, and I didn't have time for waiting on their staff on the phone (who does these days, right?), I've decided to use Twitter. I had previous experience with them via this channel, and a positive one. They seemed to be on top of any problems they receive via tweets, and are quite fast to reply.\
\

|  |
|:--:|
| <a href="/assets/images/Screen_Shot_2017-09-11_at_12.06.59.png" data-imageanchor="1" style="margin-left: auto; margin-right: auto;"><img src="/assets/images/Screen_Shot_2017-09-11_at_12.06.59.png" data-border="0" data-original-height="1274" data-original-width="1268" width="318" height="320" /></a> |
| *Translation: "Hello @TelenorBanka, did you know that emails you're sending to your clients are not encrypted in transit? Info: support.google.com/mail/answer/63..."* |

\
Just to add a bit more context to the story (or to make things even worse), the bank in question is a first ever Web based bank (at least they market themselves this way) in Serbia. You'll notice that I've included a google support <a href="https://support.google.com/mail/answer/6330403?hl=en" rel="nofollow" target="_blank">link</a> in the tweet, just to point their support in the correct direction.\
\
Initially, the bank support just ignored me. It didn't help that I don't have many followers, and that my tweet did not get any special attention. I am guessing that the request I made was a bit too much for an average support guy, or I just overwhelmed them with an unusual request. So, three days later I made another tweet, politely asking them to turn on TLS for their outgoing emails.\

|  |
|:--:|
| <a href="/assets/images/Screen_Shot_2017-09-11_at_12.11.19.png" data-imageanchor="1" style="margin-left: auto; margin-right: auto;"><img src="/assets/images/Screen_Shot_2017-09-11_at_12.11.19.png" data-border="0" data-original-height="506" data-original-width="1240" width="320" height="130" /></a> |
| *Translation: "Hello, @TelenorBanka, could you turn on TLS for outgoing emails? I am not getting emails like this from other banks"* |

I also included a remark that I'm not getting the 'crossed out red lock' on the emails from other banks in Serbia. After that, they had finally replied:\

<div>

<div class="separator" style="clear: both; text-align: center;">

<a href="/assets/images/Screen_Shot_2017-09-11_at_12.13.04.png" data-imageanchor="1"><img src="/assets/images/Screen_Shot_2017-09-11_at_12.13.04.png" data-border="0" data-original-height="522" data-original-width="1220" width="320" height="136" /></a>

</div>

</div>

|  |
|:--:|
| <a href="/assets/images/Screen_Shot_2017-09-11_at_12.13.39.png" data-imageanchor="1" style="margin-left: auto; margin-right: auto;"><img src="/assets/images/Screen_Shot_2017-09-11_at_12.13.39.png" data-border="0" data-original-height="498" data-original-width="1242" width="320" height="128" /></a> |
| *Translation: "Hi Aleksandar, to make sure all of our users get our emails, we don't have the implementation of encryption algorithms to encrypt the emails in plan at the moment. Thanks for your suggestion and patience for other users' sake. :) /Dragana"* |

\
The translated gist of their response spanning two tweets is: "we won't do it for compatibility sake". How valid is this excuse in 2017? I'll tell you: it just isn't.\
\
\
NOTE: the text above was drafted during 2017. A tweet I've seen today, shaming another major provider of services in Serbia for their lack of use of the TLS, prompted me to follow up and actually publish the post. Having a lot of time passed, I am posting a screenshot of an up to date email sent to me from the bank. As you can see from the picture below, nothing had changed:\
\

<table class="tr-caption-container" data-align="center" data-cellpadding="0" data-cellspacing="0" style="margin-left: auto; margin-right: auto; text-align: center;">
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td style="text-align: center;"><a href="/assets/images/Screen_Shot_2018-05-08_at_14.23.34.png" data-imageanchor="1" style="margin-left: auto; margin-right: auto;"><img src="/assets/images/Screen_Shot_2018-05-08_at_14.23.34.png" data-border="0" data-original-height="356" data-original-width="832" width="320" height="136" /></a></td>
</tr>
<tr>
<td class="tr-caption" style="text-align: center;"><div style="font-size: 12.8px;">
<em>As you can see, the email wasn't transferred via TLS; the subject line translates to "Transaction  approved" - and the email lists my account balance, transaction amount as well as the vendor.</em>
</div>
<div style="font-size: 12.8px;">
<div style="font-size: 12.8px;">
&#10;</div>
</div></td>
</tr>
</tbody>
</table>

\

<div>

A sad state of affairs indeed.

</div>
