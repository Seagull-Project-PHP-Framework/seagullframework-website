<!-- Name: Misc/Modularity -->
<!-- Version: 1 -->
<!-- Last-Modified: 2005/11/06 15:50:05 -->
<!-- Author: trac -->
FIXME: Look over new file... 

Seagull is an OOP application with an emphasis on modularity. The framework consists of:

  * *base framework*: The framework itself consists of a set of base classes organised according to the MVC design pattern that take care of permissions, authentication, sessions, i/o and database abstraction layer. Developers familiar with Struts and JSP deployment will recognise the approach
  * *modules*: Each generalised area of functionality comes in the form of a module, your business requirements may be matched to modules that already exist in framework. If a module doesn't exist please request it and our community of developers will take a crack.  Better yet, have your developers build it and contribute it back to the project :-) 
  * *libraries*: Most task-specific functionality comes from libraries, quite often from [PEAR](http://pear.php.net), that can be independently updated when upgrades/improvements are available 
  * *entities/entity managers*: each object in the application (Member, Group, Property, Document, Article, etc) is represented as an entity, developers are provided with tools to quickly prototype entities so that skeleton classes are created and updated automatically
