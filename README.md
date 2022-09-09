ginhouxnet.rsyslog
=========

This ansible role configure rsyslog and only use new syntax !

WARNING : It doesn't check rsyslog version, so if you're using and older version of rsyslog who don't support the new syntax, you'll get fews errors. But, this role is "locked" to be used on recent linux version, with recent rsyslog version


Requirements
------------

This role is built to only run on platforms defined in `meta/main.yml`


Role Variables
--------------

Necessary variables are defined on `defaults/main.yml`


Dependencies
------------

none


Example Playbook
----------------



License
-------

BSD


Author Information
------------------

https://github.com/dginhoux/
