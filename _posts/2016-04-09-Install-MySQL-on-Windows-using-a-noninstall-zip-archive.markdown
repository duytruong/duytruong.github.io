---
layout: post
title:  "Install MySQL on Windows using a noninstall zip archive"
date:   2016-04-09 15:20:01
categories: [mysql]
---
1. Download at (http://dev.mysql.com/downloads/mysql/) (choose (mysql-VERSION-win32.zip) or (mysql-VERSION-win64.zip) )
2. Extract zip file (e.g C:\mysql-5.7.11-win64)
3. Add C:\mysql-5.7.11-win64\bin to PATH environment variable.
4. Run mysqld --initialize in command line.
5. Run mysqld --install to install MySQL service.
6. Reset root password
> Create a text file containing the password-assignment statement on a single line. Replace the password with the password that you want to use.
> MySQL 5.7.6 and later:
> ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass';
> MySQL 5.7.5 and earlier:
> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('MyNewPass');
> Save the file. This example assumes that you name the file D:\mysql-init.txt.
> Run mysqld --init-file=D:\\mysql-init.txt in command line
> Kill mysqld process in Task Manager
7. Go to Services (search in windows start menu), then start MySQL service.
8. Login with new password. Run mysql -u root -p and enter new password
Now you can use MySQL database with command line or use a GUI client such as MySQL Workbench.
Reference: (https://www.youtube.com/watch?v=O4xXzTIcnDE)
