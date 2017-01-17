<!-- Name: TipsAndTricks/InitialiseEnvironment -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/03/29 02:22:31 -->
<!-- Author: demian -->

# Initialise Seagull Environment

If you want to prepare the Seagull environment but not start any workflow, simply call the following:


	require_once dirname(__FILE__)  . '/path/to/lib/SGL/FrontController.php';
	SGL_FrontController::init();