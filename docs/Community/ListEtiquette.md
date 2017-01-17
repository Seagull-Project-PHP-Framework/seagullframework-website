<!-- Name: Community/ListEtiquette -->
<!-- Version: 5 -->
<!-- Last-Modified: 2006/04/13 14:36:52 -->
<!-- Author: werner -->

* TOC
{:toc}

## Why this page?
This page is dedicated to improving the readability of our [Mailing List][1]. As learned from other lists, the possibility of getting a quick answer is greater if you follow list etiquette.

E.g. if you  don't start a new thread your mail is not found easily, and if you do a senseless full quote the reader gets bored - and won't answer.

## So what should I do?
### Use your real name
E.g. [Werner Krauss][2] as sender name, no nicknames please. This makes things more personal ;)
If possible use Latin characters. As the list uses the English language, it's difficult to read e.g. Arabic names

### Read the documentation
There are good [FAQ][3] and [Known Issues][4] sections in the wiki.

### Browse the list archive
Look in the [Mailing list archive - MARC][5]. Maybe your problem was discussed before.

### Start a new thread for a new question
Don't just hit "Reply"... Users with mail clients that support threaded listing will thank you ;)

Again: *PLEASE start a NEW thread for your questions!* Just don't hit reply on a mail... We will see your mail deep inside another thread!

As learned from other mailing lists, your chance to get a quick answer is much better if you start a new thread! Because it's easier to find!

How to do this? Well, simply open a blank mail and type the ML address mailto:seagull-general@lists.sourceforge.net in it.
Or even better, add this address to your address book, and if you have a good mail client, it will autocomplete it after the first characters.

Why? All messages have a unique ID in the header: `Message-ID: <dek2g6$ede$1@sea.gmane.org>`
And when you reply to a message, standard-compliant mail clients add another header line: `In-Reply-To: <430CFB22.5060300@deafzone.ch>` using the Message-ID of the message replied to.  So, even if you change the subject, mail clients displaying a folder in threaded style (showing which message is in response to which previous message) use these header lines to build the thread tree.  Consequently your "new" message will end up in an unrelated thread if you just hit "reply" to a previous message.

### Don't start a new thread for an answer
Please tell your mail client to preserve the thread if you answer to a mail in a discussion. Normally just clicking on "reply" should do the job.

### And - last but not least: be polite

## So what should I NOT do?
### No full quotings if  only a few of the original lines are relevant
This blows up the mail archive and the reader gets bored while scrolling down.  In other words, quote only what you are answering, snip out code (unless commenting it), remove trailers like badly formed signatures, mail server ads and disclaimers, SourceForge information and mailing list links.

### Don't send .vcf  v-cards
See above. Who needs this stuff, at least in mailing lists?

### Don't use autoresponders
Often, people subscribed to mailing lists just set the autoresponder of their mail client (or perhaps server) before going to holidays.
As a result, if one sends a message to the mailing list, he gets dozens of messages stating that some unknown guy is out of his office...

So, don't use such autoresponders (useless IMHO) or if they really required, please be polite enough to suspend your subscription to *all* your mailing lists before leaving.

Most mailing list managers I know allow for the temporary suspension of accounts. You can catch up with the archives...

### Don't top post
This is the practice enforced by default by most mail clients that includes the message you're replying to  with the message you're writing.  Depending on your preferences, the original email will either be above or below your response, indented.

Allowing this default behaviour is annoying because you're forced to read the entire original question without being sure of the context of the answer.

The recommended practice is to put your answers/comments near the relevant part of the original quoted message, interleaving them and preferably allowing one line of whitespace between responses, for readability:

	>> What time is it?
	
	> Just look at your clock.
	
	Don't forget to synchronize it to the correct time!
	
	>> Why do I sometime see an answer message before the question message in my inbox sorted by date?
	
	Because of bad clock settings.
Two nice quotes found in message signatures:

Q: What is one of the most annoying things on Usenet?
A: Top posters.

Q. Why is top-posting considered antisocial?
A. Because it makes conversations really hard to follow.

### Last notes
  * Avoid HTML messages if possible, it is often seen as an annoyance (too flashy, more form than content) and  a waste of space (look at source: HTML tags use lot of space, and message is often repeated in plain text for primitive readers) .
  * Make a short signature (the text automatically added at the end of the message), and if your mail client doesn't do it for you, start it with two dashes followed by a space and a new line: this is the standard indicator for a signature start, and smart mail clients (not Outlook Express...) display it in a different style (I like them grey) and automatically remove it from the quote.

## Useful Links
  * http://learn.to/quote (german only)
  * http://www.netmeister.org/news/learn2quote.html
  * http://www.catb.org/\~esr/faqs/smart-questions.html

Note: the worse I've seen was a new question made by hitting Reply to the daily digest of a mailing list (ie. a message compiling all the messages of the day), without changing the subject ("XXX ML: Daily digest of 10/11/03"), putting the question on top, failing to remove the whole quote of the digest (several KB!)... - [Philippe Lhoste][6]

[1]:	/wiki:Community/MailingList/
[2]:	/wiki:User/WernerKrauss/
[3]:	/FAQ.html
[4]:	/wiki:TroubleShooting/KnownIssues/
[5]:	http://marc.theaimsgroup.com/?l=seagull-general
[6]:	/wiki:User/PhilippeLhoste/