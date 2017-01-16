<!-- Name: RFC/sgl2/ProposedChanges -->
<!-- Version: 1 -->
<!-- Last-Modified: 2010/03/20 17:26:30 -->
<!-- Author: demian -->
No more use of static calls, but using SGL2_Registry for central object storage! This makes it possible to replace and/or subclasses core library classes without updating existing code.
For example if you want to improve translation support, you can just replace the instance under SGL2_Registry::get('translation') with the new version, and all modules accessing it will use it. Same goes for things like db, session or cache.

*So to accomplish this, we have to:*

- Remove static calls on SGL2 library classes (exception:  Registry)[[BR]]
- Make SGL2_Session an abstract base class[[BR]]
- Make SGL2_Translation an abstract base class[[BR]]

*A word about SGL2_Registry:*

- A nice feature would be to allow auto-downgrade access. I would support “sub-entries” by using a “.”.  So example: SGL2_Registry::get(‘db.session’); => no entry for db.session, strip of session, get instance for ‘db’;

*A word about SGL2_Config:*

- To make it possible to replace the configuration instance, I would like to change calls like “SGL2_Config::get(site.name’) to SGL2_Registry::get(‘config’)->site->name;  Config class should not be used statically!

*A word about Exception-Handling:*

- Add an SGL2_Exception base class(es), so you can catch SGL2 related exceptions[[BR]]
- Make error messages use vprintf, so you can translate message if you like[[BR]]

*A word about SGL2_Session class:*

- Implement session save handlers sub classes  (file, db, memcache)[[BR]]
- Remove user setup from session setup[[BR]]
- Make regenerate_id only be called every xy seconds (xy=config value)[[BR]]
- Use regenerate_id parameter to remove old session file[[BR]]

*A word about SGL2_Url class:*

- Remove dependency on Horde_Routes, but use SGL2_Router interface[[BR]]
- Remove any backward compatibility hacks[[BR]]

*A word about SGL2_Translation class:*

- Add sub class “SGL2_Translation_Array” instead of drivers[[BR]]

*A word about language handling:*

- Remove language from route[[BR]]
- Preprocess language before route parsing[[BR]]
- Language options “session, url, subdomain)[[BR]]
- Option: No language in url/subdomain  for default language[[BR]]
- Auto-discover language as possible on first request[[BR]]

*A word about database:*

- Use PEAR’s MDB2 as database driver[[BR]]
- No more support for data objects[[BR]]
- SGL_DB should be removed[[BR]]
- SGL_DB::getPagedData() has to be refactored into multiple classes (data set, pager)[[BR]]
- Add SGL2_Pager to work correctly with routing[[BR]]