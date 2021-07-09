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



## b. Install a MySQl server
### i. Use MariaDB as the database for all the services. You may choose your 
own username and passwords but make a record of it so that we may 
access them.
### ii. List the following in your GitHub
    1. A command and output that shows the hostname of your database server
    2. A command and output that reports the database server version
    3. A command and output that lists all the databases in the server
## c. Install Cloudera Manager
### i. Specifically, you MUST install CDH version 5.16.2 
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
