# MySQL
root:password
mysql -h localhost -P 3306 -u root -p

## 查看当前数据库
select database();
status;
show tables; # 在列头中

## Cancel a query
> SHOW PROCESSLIST;
> KILL <thread id>
- https://stackoverflow.com/questions/18893498/cancel-a-mysql-query

## See indexes for a table
> SHOW INDEX FROM tablename;
- https://stackoverflow.com/questions/5213339/how-to-see-indexes-for-a-database-or-table

## Join
inner join 内连接或等值连接，获取两个表中字段匹配关系的记录。
left join 左连接，获取左表的所有记录，即使右表没有对应匹配的记录。
right join 右连接，获取右表的所有记录，即使左表没有对应匹配的记录。
- http://www.runoob.com/mysql/mysql-join.html

# phpmyadmin
sudo ln -s /usr/share/phpmyadmin /var/www/phpmyadmin

Have you tried to:

sudo -H gedit /etc/apache2/apache2.conf
Then add the following line to the end of the file:

Include /etc/phpmyadmin/apache.conf

- https://askubuntu.com/questions/668734/the-requested-url-phpmyadmin-was-not-found-on-this-server
- https://help.ubuntu.com/lts/serverguide/phpmyadmin.html

## config
if /etc/phpmysqladmin/config.inc.php doesn't work, try /usr/share/phpmysqladmin/config.inc.php

## generate a create table script for an existing table
> SHOW CREATE TABLE tablename;
- https://stackoverflow.com/questions/11739014/how-to-generate-a-create-table-script-for-an-existing-table-in-phpmyadmin

Toggle Full Texts in the +Options.
- https://stackoverflow.com/questions/3090209/phpmyadmin-and-show-create-table


# Reset Password
- https://www.digitalocean.com/community/tutorials/how-to-reset-your-mysql-or-mariadb-root-password
