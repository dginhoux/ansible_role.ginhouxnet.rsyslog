ginhouxnet.rsyslog
=========

This ansible role configure rsyslog and only use new syntax !

WARNING : It doesn't check rsyslog version, so if you're using and older version of rsyslog who don't support the new syntax, you'll get fews errors. But, this role is "locked" to be used on recent linux version, with recent rsyslog version


Requirements
------------

This ansible role will only run on identified OS defined on meta/main.yml


Role Variables
--------------

Everything is in defaults/main.yml


Dependencies
------------




Example Playbook
----------------



License
-------

BSD


Author Information
------------------

Dany GINHOUX
