# Helper Commands

## Command line Helpers

### Moving around
You move between folders with the `cd` command

`cd /folder/`
We will call folders directories or directory (dir) for short.
You will find yourself remembering long old routes to places on a server

- `cd /var/www/html/`
- `cd /var/log/apache2`

Here are some handy shortcuts:
- `cd ~/` Go to you logged in users home directory
- `cd /` Go to the root computer/server directory
- `cd -` Go to the last directory you were in
- `cd ../` Go back a directory
- `cd ../../` Go back two directories (and so on)
- `cd .` leaves you where you are

As a practical example depending on the server you may often need to find the location of error logs - depending on which web software the site is on there are different locations to look
- `cd  /var/logs/apache2` will put you in the apache server error log default directory
- `cd  /var/log/nginx` will put you in the nigix server error log default directory

Moving around is one thing but you still need to  see whats in the folders

`ls` The `ls` command lists all files in a directory
```
ls
3         README.md one       two
```

`ls -a` List all files even hidden dot files 

```
ls -a
.         ..        .git      .hidden   3         README.md one       two
```

You can end up with a quick display but to see the permissions of files and organise them in a list is really helpful
`ls -l` List of files with additional informaiton
```
ls -l
total 16
-rw-r--r--  1 jonathan.carter  staff     0 17 May 10:10 3
-rw-r--r--  1 jonathan.carter  staff  6784 15 May 16:22 README.md
-rw-r--r--  1 jonathan.carter  staff     0 17 May 10:10 one
-rw-r--r--  1 jonathan.carter  staff     0 17 May 10:10 two
```

`ls -lh` List of files with their size in human readable form

Notice the order  - ``permissions` `user` `user-group` `filesize` `date modified` `name`
```
 ls -lh
total 16
-rw-r--r--  1 jonathan.carter  staff     0B 17 May 10:10 3
-rw-r--r--  1 jonathan.carter  staff   6.6K 15 May 16:22 README.md
-rw-r--r--  1 jonathan.carter  staff     0B 17 May 10:10 one
-rw-r--r--  1 jonathan.carter  staff     0B 17 May 10:10 two
```

Really this is the most common one i find myself using - combines all together
`ls -lah`

```
total 16
drwxr-xr-x   8 jonathan.carter  staff   256B 17 May 10:10 .
drwxr-xr-x  27 jonathan.carter  staff   864B 15 May 16:09 ..
drwxr-xr-x  12 jonathan.carter  staff   384B 17 May 10:14 .git
-rw-r--r--   1 jonathan.carter  staff     0B 17 May 10:10 .hidden
-rw-r--r--   1 jonathan.carter  staff     0B 17 May 10:10 3
-rw-r--r--   1 jonathan.carter  staff   7.8K 17 May 10:14 README.md
-rw-r--r--   1 jonathan.carter  staff     0B 17 May 10:10 one
-rw-r--r--   1 jonathan.carter  staff     0B 17 May 10:10 two
```

### RTFM
As you go through you will hit many differt commands and they will have multiple options - the only way for you to know the full extent of these is to read the manual.
These general open in vim so to exit type `:q`
`man ls`
`man cd`
`man man`
You can find alot of helpful information in these - if your command doesnt have one google it and see what you can find.

### Viewing and Editing Files

Now that you have found the folder you will need to read the file
`cat filename` will print the files contents onto the terminal - this can dump alot of information if its a big file
You can also open the file in a text editor if you prefer

`nano filename` or `nano error.log`
`ctrl x` and type `y` will save your changes
`ctrl c` will exit out of the file without saving your changes

Most terminals will have multiple text editors you can use - `nano` `vim` are common and popular.
> `Vim/Vi` is a bit different to other editors

### Suspend and re-open
`ctrl z` will suspend the current program - handy for a text editor you want to check something but dont want to save the file
Once your done running `fg` or foreground command will put you back into the file.

### Handy Logging

If you are faced with a very large log file sometimes it helps to check the last few lines of the file
`tail -f error.log` Is a really handy command that will open the error log file and print to the screen whatever gets appended to it.

This can be useful on a site that is broken broken as it can often store useful debug information. You can reload the broken page and see the logs realtime.
You can also dump the specified number of lines if you just want to see for example the last 100 lines.

`tail -100 error.log`

For the record  - theres also the opposite command `head` to see the top few lines of a file. (if you ever need it)

### Appending and Amending Files

Sometimes you can work with files that can be giant - gb's of size. Tail is great for the last few bits what can also be helpful is sending data from one command to another
`tail -100 error.log   > last100lines.log`
This would write the results of your tail command into a new file overwriting the file 

`tail -100 error.log   >> last100lines.log` 
This would ammend the file at the bottom

### History help

Sometimes you need to find a command you ran previously because your memory isnt all it used to be - it happens no one is perfect.
The `history` command will display all commands previously run on your system by your user.
As this dumps all its only useful if it was in the last few commands.
A really helpful trick is to press the `up` arrow on your keyboard, as this will loop through the last commands.
If you remember doing a command in the mists of time and kind of remember some of it you can pass the command into another helpful function `grep`

`history | grep searchterm`
As an example - 
`history | grep git` normally tells me what the damn command was I ran to reset my git branch to a new org

```
 9739  git remote -v
 9740  git remote set-url  origin git@github.com:dividohq/finance-plugin-woocommerce.git
 9741  git status
```
### Grep for quick searching

If you dont have a fancy IDE for your coding sometimes you will need to find a bunch of random stuff in a folder.
`grep` can help you here too

`grep -Ril woocommerce_product_write_panel_tabs ../demo_divido_co/`
This command will recursively search for files with `woocommerce_product_write_panel_tabs` in in the `../demo_divido_co/` folder.
It will dump out all files with that string in - which can help.

### A note on permissions

Who you are on the server matters - if you are logged in as root you are going to endup running a command and breaking a websites permissions
Especially if you are running some magento commands - you can recursively reset the file owner like so:
`chown -R www-data:www-data ../m2dev`
This command changes the owner of files in the m2dev dir to be owned by the default apache web user `www-data`.
`chmod 777 file` if you are getting weird permissions this means everyone can do anything to it.
You wouldnt leave your car open, with the keys in,engine running, with your pin number taped to you credit card and id docs in a high crime area so dont leave your files lucky 7 on a server.
If you turn your files into a slut to fix an issue always put them back when your done.


## Mysql Helpers

You can login with mysql on the server - obviously there are security implications to this so think about it.
`mysql -u root -p`

```
root@server:~# mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 32681
Server version: 5.7.26-0ubuntu0.16.04.1 (Ubuntu)

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>```

Once you have a mysql shell  you can exit it
`mysql> exit;`

Some basic example commands:
`mysql> SELECT * FROM table LIMIT 5`
`mysql> UPDATE wp_options SET value='this' WHERE id='that' `


### Logging in automatically without `-p`
create a empty file `touch ~/.mylogin.cnf`

```[client]
user = root
password = super-secure-password
```

Make sure you restrict access to this file!
`chmod 600 /home/example_user/.mylogin.cnf`

### BACKUP and RESTORE


### Make a full backup of all DB's
`mysqldump --all-databases --single-transaction --quick --lock-tables=false > full-backup-$(date +%F).sql -u root -p`
### Make a backup of a single DB
`mysqldump -u username -p db1 --single-transaction --quick --lock-tables=false > db1-backup-$(date +%F).sql`
### Make a backup of a single Table from a DB
`mysqldump -u username -p --single-transaction --quick --lock-tables=false db1 table1 > db1-table1-$(date +%F).sql`

### Create an automated backup on a server
`crontab -e`
`0 1 * * * /usr/bin/mysqldump --defaults-extra-file=/home/example_user/.my.cnf -u root --single-transaction --quick --lock-tables=false --all-databases > full-backup-$(date +\%F).sql`

## Restore from our backup
`mysql -u root -p < full-backup.sql`



