<!-- Name: Howto/Navigation/Addons -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/06/11 22:05:45 -->
<!-- Author: demian -->
# How to Use Navigation Addons


Go to Administrator\Navigation.
Click New section.

In the Section info:


       Section title: any text
       Parent section: User menu
       Target: addon
       Addon class name: OutputAddon
       Name of passed menu object from module: navAddon


In the Editing options:


       Publish: enabled
       Can view: All roles


In module manager create the menu item objects, see [browser:/branches/0.6-bugfix/modules/navigation/classes/addons/DemoAddon.php DemoAddon] for an example. Assign the objects to $output->navAddon.