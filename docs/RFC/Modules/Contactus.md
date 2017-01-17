<!-- Name: RFC/Modules/Contactus -->
<!-- Version: 4 -->
<!-- Last-Modified: 2006/02/04 21:17:18 -->
<!-- Author: werner -->
# RFC for Contact Us module
[[TOC]]

## Mindaugas suggestions

\~\~I don't know if it fits for Seagull in general, but for my installation I have changed line in ContactUsMgr::sendEmail() from:[[BR]]
'fromEmail'     =\> $oContact-\>email,[[BR]]
to:[[BR]]
'fromEmail'     =\> "\"{$contacterName}\" \<{$oContact-\>email}\>",[[BR]]
to see user name instead of email.\~\~


Some more things regarding 'contact us' emails:
  * There is no support for text-only 'contact us' emails. I hate html emails :-):-) Could be some option in configuration.
  * Hardcoded 'headerTemplate' and 'footerTemplate' variables should be moved from SGL\_Emailer class to template(s) to make emails more customizable.
  * Even email title from ContactUsMgr class could moved to template or at least changed from:
	'subject'       =\> SGL\_String::translate('Contact Enquiry from') .' '. $conf['site']['name'],

  to:

	'subject'       => sprintf(SGL_String::translate('contact us mail title'), $conf['site']['name']),

  and using unique mail title label not used anywhere else (at the moment 'Contact Enquiry from' label is used in email body template).

## Werners suggestions
\~\~I need to map an email adress to an enquiry type.
Also i'd like to move enqiry types out of lang-file into ini file.[[BR]]
so my suggestion for ini enqiry syntax is file is:\~\~

	[EnquiryEN]
	enquiry1 = General Enquiry
	enquiry2 = Special Enquiry
	enquiry3 = Anything
	
	[EnquiryDE]
	enquiry1 = Allgemeine Anfrage
	enquiry2 = Spezielle Anfrage
	enquiry3 = Irgendwas

\~\~Now you can add email adresses or user-ids per enquiry or per enquiry and language:\~\~

	[EmailDefault]
	default = default@bar.com
	enquiry1 = foo@bar.com

So far coded in #448

Next step would be "Email Address per enquiry type and language":


	[EmailEN]
	default = "foo@bar.com";
	enquiry3 = user:1; // use email adress of user_id 1


  * If no email Address for a language is set, the default email will be used
  * If no email Address for a enquiry is used the default email for this language will be used.

This should enable a simple but fine grained system.
