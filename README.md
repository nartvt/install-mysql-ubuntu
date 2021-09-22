**Mysql server install on ubuntu**

	sudo apt-get update
	sudo apt-get install mysql-server
	
**Add Step here If OS is fedora**

	sudo mysql_secure_installation
	systemctl status mysql.service
	[option]mysqladmin -p -u root version
	[mandatory]sudo apt-get update

**Mysql workbench install**

	1.sudo apt-get install mysql-workbench
	2.sudo apt-get update
**MySQL config service**

	start: sudo service mysql [start,stop,restart,status]
	OR
	restart: sudo systemctl [restart,stop,start,status] mysql
**Remove mysql**

	sudo apt-get remove --purge mysql*
	sudo apt-get purge mysql*
	sudo apt-get autoremove
	apt-get autoclean
	deluser mysql
	rm -rf /var/log/mysql
	rm -rf /var/lib/mysql
	rm -rf /etc/mysql
**Create more user mysql**

Login to mysql, make sure mysql available running

		mysql -u root -p sys 

**Case 1: Create an account that uses the default authentication plugin and the given password.Mark the password expired so that the user must choose a new one at the first connection to the server**

	CREATE USER 'temo'
	IDENTIFIED BY 'temo123' PASSWORD EXPIRE;
**Case 2: Create an account that uses the sha256_password authentication plugin and the given password. Require that a new password be chosen every 180 days:**

	CREATE USER 'temo'@'localhost'
	IDENTIFIED WITH sha256_password BY 'new_password'
	PASSWORD EXPIRE INTERVAL 180 DAY;

<strong>Because the capabilities of this statement were expanded considerably in MySQL 5.7.6, this section first describes current syntax, and then the more limited pre-5.7.6 syntax</strong>

**Case 3: Cretae an account that uses the default authentication plugin and the given password, password never expire**
	
	mysql> CREATE USER 'temo' IDENTIFIED BY 'temo123';

***Set permisstion superuser***

	mysql> GRANT ALL PRIVILEGES ON *.* TO 'temo' WITH GRANT OPTION;

***Password change***

	ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass';
	ALTER USER 'student'@'localhost' IDENTIFIED WITH mysql_native_password BY 'pass123';
<hr>
Mysql account: temo/temo123  <br>
Root: root/root
<hr>

## Fix error cannot access to mysql with root account: ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)

**I recently upgrade my Ubuntu 15.04 to 16.04 and this has worked for me:**

<strong>1. First, connect in sudo mysql</strong>

	sudo mysql -u root

 <strong>2. Check your accounts present in your db</strong>

	SELECT User,Host FROM mysql.user;

	+------------------+-----------+
	| User             | Host      |
	+------------------+-----------+
	| admin            | localhost |
	| debian-sys-maint | localhost |
	| magento_user     | localhost |
	| mysql.sys        | localhost |
	| root             | localhost |

<strong>3. Delete current root@localhost account</strong>

	mysql> DROP USER 'root'@'localhost';
	Query OK, 0 rows affected (0,00 sec)
	Recreate your user

	mysql> CREATE USER 'root' IDENTIFIED BY 'root';
	Query OK, 0 rows affected (0,00 sec)
	Give permissions to your user (don't forget to flush privileges)

	mysql>GRANT ALL PRIVILEGES ON *.* TO 'root' WITH GRANT OPTION;
	Query OK, 0 rows affected (0,00 sec)

	mysql>FLUSH PRIVILEGES;
	Query OK, 0 rows affected (0,01 sec)
	Exit MySQL and try to reconnect without sudo.
