<!-- Name: Integration/Paypal -->
<!-- Version: 2 -->
<!-- Last-Modified: 2006/11/30 15:52:45 -->
<!-- Author: demian -->
# Integrating Paypal

A couple of years ago I put together some quick Paypal integration code, based on a lib that was available at the time.  Grab the code [here](http://seagullfiles.phpkitchen.com/paypal.tar.gz).

The contents look like this:


    -rw-r~~r~~   1 demian demian 8294 May  3 00:07 IpnMgr.php
    -rw-r~~r~~   1 demian demian 4595 May  3 00:09 paypal_ipn.php
    -rw-r~~r~~   1 demian demian 1605 May  3 00:08 Transaction.php
    -rw-rw-r~~   1 demian demian  876 May  3 00:11 transaction.sql

Even though the code is old the Seagull basics have changed little, so you can pretty much copy and paste what's here.
  * IpnMgr.php: manages Paypal notifications, call this with a static file like example.com/paypal.php
  * paypal_ipn.php: this small class takes care of the main logic
  * Transaction.php: this is the entity created by DataObject
  * transaction.sql: here is the sql for a table you could use to store the transactions

Ideally, some of these ideas could be used and integrated into the [Payment_Process](http://pear.php.net/package/payment_process/) package at PEAR, as a Paypal driver.

[[AddComment]]