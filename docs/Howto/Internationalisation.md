<!-- Name: Howto/Internationalisation -->
<!-- Version: 17 -->
<!-- Last-Modified: 2009/03/04 10:52:18 -->
<!-- Author: demian -->
<!-- Status: Original -->

# I18N: Internationalisation
* TOC
{:toc}

 * [Start][1]
 * [General overview][2]
 * [How to setup the environment][3]
 * [Best practices][4]
 * [How to convert your Seagull site to utf-8][5]
 * [RTL support][6]
 * [Submitting translations][7]

Seagull makes it easy to translate the application GUI. See also: Howto/Internationalisation/TranslatingModules. Admins can customise the presentation time/date/numerical formats based on their customer's locale, ie, geographic location.

## Supported Languages

[wiki:Community/Maintainers/Translations]

The current language designation is held in a session variable and can be switched on a page-by-page or session basis.

## Non-english sites

If you want to use a czech language pack czech-iso-8859-2 instead of default english then

  *  just for you: login as a member, edit your profile, and set Czech as your interface language
  *  for all users: login as an admin and select General -\> Config, then choose the 'translation' tab and set the fallback language to Czech.

## Language Files
All translatable strings are stored in easy to manage language files, containing a simple PHP array:


	    $words = array(
	/*  FAQ */
	        'FAQs' => 'FAQs',
	        'FAQ Manager' => 'FAQ Manager',
	        'FAQ Manager :: Browse' => 'FAQ Manager :: Browse',
	        'Please fill in a question' => 'Bitte geben Sie eine Frage ein',
	        'Please fill in an answer' => 'Bitte geben Sie die dazugeh&#246;rige Antwort ein',
	
	        'Question' => 'Frage',
	        'Answer'   => 'Antwort',
	
	        'FAQ ID'   => 'FAQ ID',
	        'Date created' => 'Erstellungsdatum',
	
	        //list
	        'Questions' => 'Fragen',
	        'Answers' => 'Antworten',
	    );

You will find these files in the lang folder of each module, where the filename is [language\_name]-[encoding].php

The language files locations:
  * module-specific: /modules/your-module-name/lang/
  * default: /modules/default/lang
The default language file is loaded on every request, so if you have common strings that appear within many modules, put them here.

## Switching Language with Request Params
Either POST or GET are fine, to switch just using the URI pass the lang param like this:


	http://example.com/about/?lang=ru-windows-1251

## Translation of strings in templates
Developers should avoid translating strings inside the managers, the right place for this is in the templates.

	<tr>
	    <th align="left">{translate(#Question#)}:</th>
	    <td>
	        <div class="error" flexy:if="error[question]">
	            {translate(error[question])}
	        </div><textarea cols="40" rows="4" name="faq[question]">{faq.question}</textarea>
	    </td>
	</tr>

Use 

	{translate(pageTitle)}

for variables passed to $output and


	{translate(#Question#)}

for text in templates (e.g. for the GUI)

## Locale
By default Seagull will provide basic PHP locale functionality, ie, you can call PHP functions in your code like strftime() and they will  be modified by the set locale.

To take advantage of advanced locale handling go to Config screen and choose:


	General Site Options -> Extended locale support -> On

This integrates Seagull with PEAR's [I18Nv2][8] library, an example of usage:


	$locale =& SGL_Locale::singleton();
	
	echo $locale->formatCurrency(2000,I18Nv2_CURRENCY_LOCAL);
	echo $locale->formatDate(time());

See [source:/trunk/lib/SGL/Locale.php] for the implementation.

In Config, 'localeCategory' is configurable, it's set to LC\_ALL by default although European users will want to change this where supplying a ',' for the decimal separator causes calculation probs (use LC\_TIME)

## Timezone hack
Good timezone hack info here:

http://www.geeklog.net/forum/viewtopic.php?forum=10&showtopic=21232

## Translating Navigation
To get also menus and breadcrumbs translated translations have to be stored in the database.

[1]:	/wiki:Howto/Internationalisation/
[2]:	/wiki:Howto/Internationalisation/General/
[3]:	/wiki:Howto/Internationalisation/TechSetup/
[4]:	/wiki:Howto/Internationalisation/TranslationBestPractices/
[5]:	/wiki:Howto/Internationalisation/ConvertingSeagullSitesToUtf8/
[6]:	/wiki:Howto/Internationalisation/HebrewAndRtlLanguages/
[7]:	/wiki:Howto/Internationalisation/SubmittingTranslations/
[8]:	http://pear.php.net/package/I18Nv2/