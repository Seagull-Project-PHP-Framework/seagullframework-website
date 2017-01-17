<!-- Name: Standards/HighlyCohesiveLooselyCoupledClasses -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/04/01 22:27:02 -->
<!-- Author: demian -->

# Highly Cohesive and Loosely Coupled Classes

Thanks to these notes on [Program Design][1] for the following words about the sound OO design principle of building highly cohesive, loosely coupled components:

Your set of classes should be loosely coupled, but each class should show high cohesion. Coupling is a measure of the interdependence between classes. If every object has a reference to every other object, then there is high coupling, and this is undesirable because there's potentially too much information flow between objects. Low coupling is desirable: it means that objects work more independently of each other. Low coupling minimises the "ripple effect" where changes in one class cause necessity for changes in other classes.

Cohesion is a measure of strength of the association of variables and methods within a class. High cohesion is desirable because it means the class does one job well. Low cohesion is bad because it indicates that there are elements in the class which have little to do with each other. Modules whose elements are strongly and genuinely related to each other are desired. Each method should also be highly cohesive. Most methods have only one function to perform. Don't add 'extra' instructions into methods that cause it to perform more than one function. 

Additionally see this detailed explanation: [http://c2.com/cgi/wiki?CouplingAndCohesion][2].

[1]:	http://www.cs.bham.ac.uk/~mdr/teaching/modules02/java2/design.html
[2]:	http://c2.com/cgi/wiki?CouplingAndCohesion