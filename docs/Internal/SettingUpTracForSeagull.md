<!-- Name: Internal/SettingUpTracForSeagull -->
<!-- Version: 3 -->
<!-- Last-Modified: 2005/11/22 04:00:02 -->
<!-- Author: aj -->

Copy & Paste the blow commands in to trac-admin to setup a project that uses seagull.

component remove component1

component remove component2

version remove 1.0

version remove 2.0

milestone remove milestone1

milestone remove milestone2

milestone remove milestone3

milestone remove milestone4

priority remove blocker

priority remove critical

priority remove major

priority remove minor

priority remove trivial

permission remove anonymous TICKET\_CREATE

permission remove anonymous TICKET\_MODIFY

permission remove anonymous WIKI\_CREATE

permission remove anonymous WIKI\_MODIFY

priority add highest

priority add high

priority add normal

priority add low

priority add lowest

priority add blocked

severity add critical

severity add major

severity add normal

severity add minor

severity add trivial

component add 'SGL - AppController' somebody

component add 'SGL - BlockLoader'     somebody

component add 'SGL - Categroy' somebody

component add 'SGL - CategroyPerms' somebody

component add 'SGL - Config' somebody

component add 'SGL - DB' somebody

component add 'SGL - Download' somebody

component add 'SGL - Emailer' somebody

component add 'SGL - ErrorHandler' somebody

component add 'SGL - HTTP' somebody

component add 'SGL - Installer' somebody

component add 'SGL - Item' somebody

component add 'SGL - Locale' somebody

component add 'SGL - Manager' somebody

component add 'SGL - NestedSet' somebody

component add 'SGL - Output' somebody

component add 'SGL - ParamHandler' somebody

component add 'SGL - Registry' somebody

component add 'SGL - Request' somebody

component add 'SGL - Sql' somebody

component add 'SGL - String' somebody

component add 'SGL - Url' somebody

component add 'SGL - Util' somebody

component add 'SGL - Wizard' somebody

component add 'Documentation' somebody

component add 'Modules - Blocks' somebody

component add 'Modules - ContactUs' somebody

component add 'Modules - Default' somebody

component add 'Modules - Documentor' somebody

component add 'Modules - Export' somebody

component add 'Modules - FAQ' somebody

component add 'Modules - Guestbook' somebody

component add 'Modules - Messaging' somebody

component add 'Modules - Navigation' somebody

component add 'Modules - Newsletter' somebody

component add 'Modules - Publisher' somebody

component add 'Modules - RandomMsg' somebody

component add 'Modules - User' somebody

component add 'Not Categorized' somebody

component add 'Theme - Default' somebody

permission add authenticated TICKET\_CREATE

permission add authenticated TICKET\_MODIFY

permission add authenticated WIKI\_CREATE

permission add authenticated WIKI\_DELETE

permission add authenticated WIKI\_MODIFY