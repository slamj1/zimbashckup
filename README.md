zimbashckup
===========

ZimBashckup : Zimbra Backup Script in Bash
------------------------------------------

About
-----
Simple Bash script for backing up zimbra mailboxes.  
Inspired by the script zimbraBackupAllAccounts.sh written by Richardson Lima and available here : http://wiki.zimbra.com/wiki/Backing_up_and_restoring_Zimbra_%28Open_Source_Version%29#Another_option  
I modified Richardson's script for personal needs, and I think it could help people, so I decided to redistribute it.

Requirements
------------
- bash
- gawk
- date
- Zimbra Collaboration Suite (tested on the Open Source Edition v8.0.6, 8.0.7, 8.5.0 and 8.6.0 on Ubuntu 12.04 and 14.04)

Installation
------------
- Give the correct rights in order to execute the zimbashckup.sh from the zimbra user (install it in /usr/local/bin and "chmod a+x /usr/local/bin/zimbashckup.sh" should work).

Default behavior
----------------
- Without arguments, it will backup all the mailboxes using the tar format with folders in separates tarballs (it launches a "zmmailbox getRestUrl" per folder).
- Empty folders are ignored and won't be backed up.
- Empty mailboxes will only have their folder created, but nothing backed up.
- Shared folders from another account will only be backed up for the owner account. For the others accounts, the folder will be ignored.
- Mail filters are backed up in sieve format
-- Even if you don't have any filter set in your account, the zimbra tool exports a 2 lines file, I decided not to drop those 2 lines. So the file will always be created.

Argument/options list
---------------------
- -v/--verbose : Verbose/debug mode, displays the status of the current task running.
- -u/--unite : Backup whole mailbox at the time (one file by mailbox). Faster, but uses a lot of CPU/RAM and you won't be able to restore only a folder if needed.
- -p[cmd]/--postscript=[cmd] : Run the script given after the backup.
- -m[mailboxes]/--mailboxes=[mailboxes] : Backup only this/these mailboxes (each mailbox separated by a space).
- -d[domains]/--domains=[domains] : Backup only mailboxes which belong to this/these domains (each domain separated by a space).
-- The options -m and -d can't be used alltogether.
- -f[tar|tgz|zip]/--format=[tar|tgz|zip] : Format used to store backups (this is given to zmmailbox getRestURL command). Default is tar.
- --dest : Destination folder (defaut is "/opt/zimbra/backups"). Needs to be writable by the zimbra user.
- -k[number of days]/--keep=[number of days] : Retention time in days
- -l[locale]/--locale=[locale] : Locale used for the backup process (default is "en_US.UTF-8"). Useful when backing up mailboxes with special characters in their names.

Examples
--------

Backup mailboxes from domain "domain1.tld" using the tgz format and using one file per mailbox : 
	zimbashckup.sh --domains="domain1.tld" --format=tgz --unite


Backup only mailboxes user1@domain1.tld and user1@domain2.tld with one file by folder using format zip :
	zimbashckup.sh --mailboxes="user1@domain1.tld user1@domain2.tld" --format=zip

Backup all mailboxes one file by folder using format tgz, use the locale "fr_FR.UTF-8", keep 5 days of backups and put them in /var/backups/zimbra  :
	zimbashckup.sh --format=tgz --locale="fr_FR.UTF-8" --keep=5 --dest="/var/backups/zimbra"

Restoring Backups
-----------------
For now, Zimbashckup doesn't do mailbox restore.

For the restore process, it has to be performed with the zmmailbox tool (http://wiki.zimbra.com/wiki/Zmmailbox) with the postRestURL command.

Something like `/opt/zimbra/bin/zmmailbox -z -m user@domain.com -t 0 postRestURL "//?fmt=tgz&resolve=reset" account.tgz` should work, but be carefull (mostly with the resolve parameter, when set to "reset", it will empty the destination mailbox and replace it with the TGZ dump.

If you can work with a test zimbra, don't hesitate to test it, I've done a lot of mistakes when testing.

License
-------
Zimbashckup is distributed under the terms of the GNU General Public  
License as published by the Free Software Foundation; either version 2  
of the License, or (at your option) any later version.  A copy of this  
license can be found in the file LICENSE included with the program.  

Ideas, questions, patches and bug reports
-----------------------------------------
Send me a mail, and I will answer you, :).

--
2013-2014 by Cyril Lavier
bainisteoir(at)davromaniak(dot)eu
