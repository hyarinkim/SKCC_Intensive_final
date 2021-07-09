# 1. Create a CDH Cluster on AWS
```
호스트1	15.164.133.134	10.0.0.52   cm
호스트2	15.164.183.255	10.0.0.5    mn 
호스트3	15.164.26.247	10.0.0.15   dn1 
호스트4	15.164.31.40	10.0.0.18    dn2
호스트5	15.165.139.112	10.0.0.20    dn3
```

## a. Linux setup
### i. Add the following linux accounts to all nodes
    1. User training with a UID of 8800
    2. Set the password for user “training” to “training”
    3. Create the group skcc and add training to it
    4. Give training sudo capabilities
```
sudo useradd training
sudo usermod -u 8800 training
sudo passwd training
sudo groupadd skcc
cat /etc/passwd | grep training
sudo usermod -aG skcc training
sudo usermod -aG wheel training
```
![a-1](https://user-images.githubusercontent.com/83220832/125042572-f26f4880-e0d4-11eb-91ef-50c926492326.PNG)

### ii. List the your instances by IP address and DNS name (don’t use /etc/hostsfor this)
```
호스트1	15.164.133.134	10.0.0.52   cm
호스트2	15.164.183.255	10.0.0.5    mn 
호스트3	15.164.26.247	10.0.0.15   dn1 
호스트4	15.164.31.40	10.0.0.18    dn2
호스트5	15.165.139.112	10.0.0.20    dn3
```
### iii. List the Linux release you are using
```
 grep . /etc/*release
```
![a-2](https://user-images.githubusercontent.com/83220832/125042986-71fd1780-e0d5-11eb-9490-a3033a413c25.PNG)


### iv. List the file system capacity for the first node (master node)
```
df -TH
```
![a-3](https://user-images.githubusercontent.com/83220832/125043060-85a87e00-e0d5-11eb-9129-826536e257fa.PNG)


### v. List the command and output for yum repolist enabled
```
yum repolist all
```
![a-4](https://user-images.githubusercontent.com/83220832/125043209-ae307800-e0d5-11eb-898b-70c397d0f90f.PNG)

### vi. List the /etc/passwd entries for training (only in master name node)
```
cat /etc/passwd | grep training
```
![a-5](https://user-images.githubusercontent.com/83220832/125043439-e2a43400-e0d5-11eb-80f7-ad75ae167ba8.PNG)

### vii. List the /etc/group entries for skcc (only in master name node)
```
cat /etc/group | grep skcc
```
![a-6](https://user-images.githubusercontent.com/83220832/125043508-f8195e00-e0d5-11eb-9a15-cbfc37964a12.PNG)

### viii. List output of the flowing commands:
    1. getent group skcc
    2. getent passwd training
 ```
getent group skcc 
getent passwd training
```
   ![a-7](https://user-images.githubusercontent.com/83220832/125043590-0ebfb500-e0d6-11eb-8c0e-b9c6f75ec37c.PNG)

>> 나머지는 CHD Install guiede 따라서 진행 

## b. Install a MySQl server
1) install MariaDB on cm
```
sudo yum install -y mariadb-server
sudo systemctl enable mariadb
sudo systemctl start mariadb
sudo /usr/bin/mysql_secure_installation
```
*비밀번호 설정 (id:root/pw:admin)
2) install 확인 및 접속
```
 mysql -u root -p
 + password 입력
```
3) create databases & user
```
CREATE DATABASE scm DEFAULT CHARACTER SET utf8 DEFAULT COLLATE 
utf8_general_ci;
GRANT ALL ON scm.* TO 'scm-user'@'%' IDENTIFIED BY 'somepassword';
CREATE DATABASE amon DEFAULT CHARACTER SET utf8 DEFAULT COLLATE 
utf8_general_ci;
GRANT ALL ON amon.* TO 'amon-user'@'%' IDENTIFIED BY 'somepassword';
CREATE DATABASE rman DEFAULT CHARACTER SET utf8 DEFAULT COLLATE 
utf8_general_ci;
GRANT ALL ON rman.* TO 'rman-user'@'%' IDENTIFIED BY 'somepassword';
CREATE DATABASE hue DEFAULT CHARACTER SET utf8 DEFAULT COLLATE 
utf8_general_ci;
GRANT ALL ON hue.* TO 'hue-user'@'%' IDENTIFIED BY 'somepassword';
CREATE DATABASE metastore DEFAULT CHARACTER SET utf8 DEFAULT COLLATE 
utf8_general_ci;
GRANT ALL ON metastore.* TO 'metastore-user'@'%' IDENTIFIED BY 
'somepassword';
CREATE DATABASE sentry DEFAULT CHARACTER SET utf8 DEFAULT COLLATE 
utf8_general_ci;
GRANT ALL ON sentry.* TO 'sentry-user'@'%' IDENTIFIED BY 'somepassword';
CREATE DATABASE oozie DEFAULT CHARACTER SET utf8 DEFAULT COLLATE 
utf8_general_ci;
GRANT ALL ON oozie.* TO 'oozie-user'@'%' IDENTIFIED BY 'somepassword';
GRANT ALL ON test.* TO 'eduuser'@'%' IDENTIFIED BY 'letmein';
FLUSH PRIVILEGES;
SHOW DATABASES;
EXIT;
```

### ii. List the following in your GitHub
    1. A command and output that shows the hostname of your database server
```    
hostname
```   
![a-8](https://user-images.githubusercontent.com/83220832/125044137-a45b4480-e0d6-11eb-9533-e266b9714c3c.PNG)
    
    2. A command and output that reports the database server version
``` 
mysql --version
``` 
![a-9](https://user-images.githubusercontent.com/83220832/125044240-bd63f580-e0d6-11eb-9ad0-e3ac694afa03.PNG)

    3. A command and output that lists all the databases in the server
 ``` 
 show databases;
 ``` 
![a-10](https://user-images.githubusercontent.com/83220832/125044367-dc628780-e0d6-11eb-8028-c44e60f66e3d.PNG)


## c. Install Cloudera Manager
### i. Specifically, you MUST install CDH version 5.16.2 

![a-11](https://user-images.githubusercontent.com/83220832/125046458-fef5a000-e0d8-11eb-972e-bc6a890574e5.PNG)
![a-12](https://user-images.githubusercontent.com/83220832/125047400-e6d25080-e0d9-11eb-85f6-137018ab7530.PNG)
![a-13](https://user-images.githubusercontent.com/83220832/125047687-2ef17300-e0da-11eb-9329-52d7e2de6b4a.PNG)
![a-14](https://user-images.githubusercontent.com/83220832/125048177-9d363580-e0da-11eb-9dbc-951394f3f40e.PNG)
![a-15](https://user-images.githubusercontent.com/83220832/125048325-c22aa880-e0da-11eb-89cb-4bf3c45fd4bc.PNG)
![a-16](https://user-images.githubusercontent.com/83220832/125048329-c35bd580-e0da-11eb-80eb-449d2cb32ed2.PNG)
![a-17](https://user-images.githubusercontent.com/83220832/125048602-0f0e7f00-e0db-11eb-930f-aa72f73d4b1a.PNG)
![a-18](https://user-images.githubusercontent.com/83220832/125048851-4bda7600-e0db-11eb-9ade-b150ce015266.PNG)
![a-19](https://user-images.githubusercontent.com/83220832/125048951-6876ae00-e0db-11eb-8b24-ddef354f8539.PNG)


### ii. The Cluster does not have to be in HA mode.
### iii. Make sure that the following services (and any necessary services to install that service) are installed:
    1. HDFS
    2. YARN
    3. Sqoop
    4. Hive
    5. Impala
    6. HUE
### iv. In you cluster, create a user named “training” with password “training”
    1. You should have already created the linux user from previous step. Now, make sure user “training” has both a linux and HDFS home directory



# 여기까지 했습니다.. 시간이 너무 부족했어요 ㅠㅠㅠㅠ...
