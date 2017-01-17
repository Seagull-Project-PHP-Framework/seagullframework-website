<!-- Name: RFC/Modules/Newsletter -->
<!-- Version: 5 -->
<!-- Last-Modified: 2007/10/24 13:00:21 -->
<!-- Author: werner -->

# Newsletter improvement

I want to include this functionality to the Newsletter module

  * \~\~subscribe/unsubscribe possibility for anybody (a separate DB only containing name, e-mail)\~\~
  * \~\~support multiple newsletter lists (Ex. subscribe to: general, developer etc..)\~\~
  * \~\~email confirmation for subscribe/unsubscribe\~\~
  * \~\~e-mail list administration for admin\~\~
  * list e-mails from a list separated with ; so you can copy/paste them in your e-mail client

  * archive for newsletters 
  * html/text-only newsletter possible [/WernerKrauss WernerKrauss] /08.02.2005 14:32/

  * archive maybe linked to publisher [/RaresBenea RaresBenea] /08.02.2005 14:42/
	* send articles of a category weekly to subscribers, maybe just "preview" text or first 30 words...

## send mails via php-cli
  * This is important for sending very high numbers of mails and avoid apaches 30 second limitation for php scripts.
  * To do this, the text for the newsletter must be saved somewhere (var/tmp/newsletter/newsletter\_id ?)
  * A db-table with all mail adresses (subscriber-ids), newsletter\_id and status (to\_send, sent) to retrieve information what mails are still to send.