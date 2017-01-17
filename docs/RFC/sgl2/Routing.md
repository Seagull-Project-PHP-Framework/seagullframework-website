<!-- Name: RFC/sgl2/Routing -->
<!-- Version: 1 -->
<!-- Last-Modified: 2010/03/19 12:01:06 -->
<!-- Author: demian -->

SGL2 should provide an abstract Routing interface to hook in Horde\_Routes or any other routing library. (like Zend\_Router) Routes should be set up per module as config file. Functionality should be kept simple, so it's not too specific to one library.

The requirements are:

\'''- SGL2\_Router is an abstract base class[[BR]]
- SGL2\_Router\_Horde is the implementation for routing with Horde\_Routes[[BR]]
- SGL2\_Router\_Condition is an abstract base class for setting a condition function[[BR]]
- Routes are translatable[[BR]]
- Routes' parameter names are translatable[[BR]]
- Make it possible to have parameters at url like ".../paramName1/paramValue1/"[[BR]]
- Make it possible to define a route to use https[[BR]]
- Force every route to have an unique name (per module)[[BR]]
- Final backlash should not be added by default to generate routes to (dynamic) files[[BR]]
- Routes setup by config file[[BR]]'''

The configuration file format is straight forward: (first comes the route name)


	login.type = static
	login.route = "/login/"
	login.defaults.module = user
	login.defaults.controller = login
	login.defaults.action = default

will match:[[BR]]
/login/[[BR]]
will not match:[[BR]]
/login/paramName1/paramValue1/paramName2/paramValue2/[[BR]]


	archive.type = static
	archive.route = "/archive/:year/:month/"
	archive.defaults.module = news
	archive.defaults.controller = archive
	archive.defaults.action = default
	archive.defaults.year = false
	archive.defaults.month = false
	archive.requirements.year = "\d+"
	archive.requirements.month = "\d+"

will match:[[BR]]
/archive/[[BR]]
/archive/2008/[[BR]]
/archive/2008/01/[[BR]]
will not match:[[BR]]
/archive/argh/[[BR]]
/archive/2008/argh/[[BR]]
/archive/2008/01/paramName1/paramValue1/[[BR]]



	search.type = dynamic
	search.route = "/search/"
	search.defaults.module = user
	search.defaults.controller = search
	search.defaults.action = default

will match:[[BR]]
/search/[[BR]]
/search/paramName1/paramValue1/paramName2/paramValue2/[[BR]]

The "dynamic" type will add "/\*params" to the end of the route and then transform it to paramName=\>paramValue.

When loading routes per module, the route name will be "moduleName.routeName" =\> makeLink('user.login');

Static routes come first, dynamic routes come last. This will not solve the problem with miss-matching completely, so always try to use requirements where possible.

The default routes are always in English and will loaded first. Only routes that are defined here, can be translated. So you cannot add routes in the translation file. Translation is just another config file like "routes.de.ini":


	archive.translation.archive = archiv
	archive.translation.year = jahr
	archive.translation.month = monat

I like the way Zend marks dynamic translatable parts within the route (if not parameter name). Just add an "@" to the beginning. So for the archive route, this means:


	archive.route = "/@archive/:year/:month/"

Another way would be to keep the route as it is without marking translatable parts, and just overwrite the route within the translation file:


	archive.route = "/archiv/:year/:month/"
	archive.translation.year = jahr
	archive.translation.month = monat

The problem about this is, that you also have to write down the parameters again, so when default route changes, translated routes have to be patched as well.

Of course, a routing cache file per language will be created for fast access.

Other options are:


	myroute.condition = SGL2_MyRouteConditionClass
	myroute.protocol = https

Be sure, that the condition class can be loaded by autoloader!