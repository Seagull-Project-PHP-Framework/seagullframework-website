<!-- Name: RFC/LiveUser -->
<!-- Version: 22 -->
<!-- Last-Modified: 2006/12/30 22:45:37 -->
<!-- Author: demian -->

# LiveUser

## Draft todo list
1. Clean up all functionality in the Seagull Core related to user/rights implementation. 
  2. Switch to the use of abstract classes.
  3. What kind of pattern should be used:

	* Bridge (This seems to be the correct pattern. We have template engines that are using bridge pattern. - Andrey)
	* Template
	* Strategy
	* Abstract Factory
2. Create a Bridge API which will be used by the SGL Core. [[BR]]
3. Implement the current SGL user/rights functionality for BC based on Bridge API and a feature that allows for the creation of an application/site without any users with the exception of an admin and guest user.[[BR]]
4. Implement LU bridge [[BR]]
5. Create Admin GUI templates for LU Managers. [[BR]]

## Tasks
1-3 - Andrey [[BR]]
4-5 - AJ

# Adapted LiveUser Models
Thanks for the great graphics from GVN Group [http://www.gvngroup.be/doc/LiveUser/index.php]
For full size images, please click the links at the bottom of the page.
== Simple Level: == 
Assign rights directly to users

The simple level of permission is efficient for managing simple applications with few users.

With an increasing number of users and complexity of applications in terms of functional areas and rights, you will want to implement higher level of permission management.

### The functional view

A first way of doing is by assigning directly a right to a user. This is an easy way of handling permissions. In this case we'll only use a subset of all LiveUser concepts.[[BR]]

[[Image(SimpleLU.gif)]]

### The technical view

Now we can slide from the functional point of view into the technical point of view.

The following graph illustrates what are the LiveUser tables and the links that we need to build for assigning rights directly to a user.[[BR]]
[[Image(SimpleLU\_Tech.gif)]][[BR]]
---- 
== Medium Level: == 
Assign rights to groups of users

An increasing number of users of a system will quickly limit the usage of the simple level of permissions. Assigning rights to a group of users requires less work than repeating the task for each user.

### The functional view
In addition to all concepts described in the simple level, we can now also deal with group of users.[[BR]]
[[Image(MediumLU.gif)]][[BR]]

### The technical view

Rights are assigned to groups. Users are made member of groups.

On a technical point of view, 2 more tables are used to make those links: «liveuser\_grouprights» and «liveuser\_groupusers».[[BR]]
[[Image(MediumLU\_Tech.gif)]]
---- 

== Complex Level: == 
Advanced Features

When working with the next level of permission management, in addition to all concepts described in the simple and medium levels, the implied rights feature can also be used. 

### The functional view
The difference with the medium level is that:

  * when a right is granted, the user may also automatically be granted another right. This is called an «implied right».
  * sub-groups are also supported[[BR]]
[[Image(ComplexLU.gif)]]

### The technical view
On a technical point of view, a new table is used to define the implied rights.[[BR]]
[[Image(ComplexLU\_Tech.gif)]]

# Seagull Version of the LiveUser ERD
Includes Sequence tables.[[BR]]
[[Image(LiveUser\_SGL\_ERD.png)]]

= LiveUser Authentication Process Sequence Diagram =[[BR]]
[[Image(LiveUserSequence.png)]]

= LiveUser Authentication Process Activity Diagram =[[BR]]
[[Image(LiveUserActivity.png)]]