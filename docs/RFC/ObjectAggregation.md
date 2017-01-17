<!-- Name: RFC/ObjectAggregation -->
<!-- Version: 2 -->
<!-- Last-Modified: 2005/11/13 19:34:47 -->
<!-- Author: werner -->

The current method of aggregating object for the output class has a few probs:

  * aggregate\_obj\_methods() works only in PHP4
  * the Reflection api stuff from PHP5 fails with a method\_exists() call

Some interesting ideas over at http://wact.sourceforge.net/index.php/ResolveHandle