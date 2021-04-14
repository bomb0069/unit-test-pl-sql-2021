# Working Oracle with Docker

## Build Container Image

```sh
sh buildContainerImage.sh -x -v 11.2.0.2 -o --memory=1g -o --memory-swap=2g
```

```sh
Checking Docker version.
Ignored MD5 sum, 'md5sum' command not available.
==========================
Container runtime info:
Client:
 Context:    default
 Debug Mode: false
 Plugins:
  app: Docker App (Docker Inc., v0.9.1-beta3)
  buildx: Build with BuildKit (Docker Inc., v0.5.1-docker)
  scan: Docker Scan (Docker Inc., v0.5.0)

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 11
 Server Version: 20.10.5
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Native Overlay Diff: true
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: runc io.containerd.runc.v2 io.containerd.runtime.v1.linux
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 269548fa27e0089a8b8278fc4fc781d7f65a939b
 runc version: ff819c7e9184c13b7c2607fe6c30ae19403a7aff
 init version: de40ad0
 Security Options:
  seccomp
   Profile: default
 Kernel Version: 4.19.121-linuxkit
 Operating System: Docker Desktop
 OSType: linux
 Architecture: x86_64
 CPUs: 2
 Total Memory: 3.847GiB
 Name: docker-desktop
 ID: KQCB:WWSP:GGMH:A7G7:LCWU:A4T3:K275:G2UM:OI7J:GFVS:NHY5:YWGI
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 HTTP Proxy: gateway.docker.internal:3128
 HTTPS Proxy: gateway.docker.internal:3129
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false

==========================
Building image 'oracle/database:11.2.0.2-xe' ...
[+] Building 74.2s (8/8) FINISHED                                                                                                              
 => [internal] load build definition from Dockerfile.xe                                                                                   0.0s
 => => transferring dockerfile: 40B                                                                                                       0.0s
 => [internal] load .dockerignore                                                                                                         0.0s
 => => transferring context: 2B                                                                                                           0.0s
 => [internal] load metadata for docker.io/library/oraclelinux:7-slim                                                                     1.5s
 => CACHED [1/3] FROM docker.io/library/oraclelinux:7-slim@sha256:f42d5807391d97b3d82c2660fcf5bf9b1207192b3cd2ab6da0a59439cfa279fa        0.0s
 => [internal] load build context                                                                                                         0.0s
 => => transferring context: 623B                                                                                                         0.0s
 => [2/3] COPY oracle-xe-11.2.0-1.0.x86_64.rpm.zip xe.rsp runOracle.sh setPassword.sh checkDBStatus.sh /install/                          2.4s
 => [3/3] RUN yum -y install unzip libaio bc initscripts net-tools openssl compat-libstdc++-33 &&     rm -rf /var/cache/yum &&     cd $  63.3s
 => exporting to image                                                                                                                    6.8s
 => => exporting layers                                                                                                                   6.8s
 => => writing image sha256:413c478d113f748b890c71c2ccc18b86d9f1c14055bca343fb13798e5ade2bdb                                              0.0s 
 => => naming to docker.io/oracle/database:11.2.0.2-xe                                                                                    0.0s 
                                                                                                                                               
                                                                                                                                               
  Oracle Database container image for 'xe' version 11.2.0.2 is ready to be extended: 
    
    --> oracle/database:11.2.0.2-xe

  Build completed in 75 seconds.
  ```

## Run Oracle Docker

`See` [FAQ](https://github.com/oracle/docker-images/blob/main/OracleDatabase/SingleInstance/FAQ.md)

### Not Work

```sh
docker run --name oracle-11c \
-p 1521:1521 -p 5500:5500 \
-e ORACLE_SID=bomb0069 \
-e ORACLE_PDB=oracle \
-e ORACLE_PWD=password \
-v /Users/lab/database/oracle/oradata:/opt/oracle/oradata \
--shm-size=1g  oracle/database:11.2.0.2-xe
```

```sh
ORACLE PASSWORD FOR SYS AND SYSTEM: 20ae71a7732841fd

Oracle Database 11g Express Edition Configuration
-------------------------------------------------
This will configure on-boot properties of Oracle Database 11g Express 
Edition.  The following questions will determine whether the database should 
be starting upon system boot, the ports it will use, and the passwords that 
will be used for database accounts.  Press <Enter> to accept the defaults. 
Ctrl-C will abort.

Specify the HTTP port that will be used for Oracle Application Express [8080]:
Specify a port that will be used for the database listener [1521]:
Specify a password to be used for database accounts.  Note that the same
password will be used for SYS and SYSTEM.  Oracle recommends the use of 
different passwords for each database account.  This can be done after 
initial configuration:
Confirm the password:

Do you want Oracle Database 11g Express Edition to be started on boot (y/n) [y]:
Starting Oracle Net Listener...Done
Configuring database...y
Done
Starting Oracle Database 11g Express Edition instance...Done
Installation completed successfully.

SQL*Plus: Release 11.2.0.2.0 Production on Tue Apr 13 08:02:50 2021

Copyright (c) 1982, 2011, Oracle.  All rights reserved.

Connected to an idle instance.

SQL> BEGIN DBMS_XDB.SETLISTENERLOCALACCESS(FALSE); END;

*
ERROR at line 1:
ORA-01034: ORACLE not available
Process ID: 0
Session ID: 0 Serial number: 0


SQL> SQL>       ALTER DATABASE ADD LOGFILE GROUP 4 ('/u01/app/oracle/oradata/bomb0069/redo04.log') SIZE 50m
*
ERROR at line 1:
ORA-01034: ORACLE not available
Process ID: 0
Session ID: 0 Serial number: 0


SQL>       ALTER DATABASE ADD LOGFILE GROUP 5 ('/u01/app/oracle/oradata/bomb0069/redo05.log') SIZE 50m
*
ERROR at line 1:
ORA-01034: ORACLE not available
Process ID: 0
Session ID: 0 Serial number: 0


SQL>       ALTER DATABASE ADD LOGFILE GROUP 6 ('/u01/app/oracle/oradata/bomb0069/redo06.log') SIZE 50m
*
ERROR at line 1:
ORA-01034: ORACLE not available
Process ID: 0
Session ID: 0 Serial number: 0


SQL>       ALTER SYSTEM SWITCH LOGFILE
*
ERROR at line 1:
ORA-01034: ORACLE not available
Process ID: 0
Session ID: 0 Serial number: 0


SQL>       ALTER SYSTEM SWITCH LOGFILE
*
ERROR at line 1:
ORA-01034: ORACLE not available
Process ID: 0
Session ID: 0 Serial number: 0


SQL>       ALTER SYSTEM CHECKPOINT
*
ERROR at line 1:
ORA-01034: ORACLE not available
Process ID: 0
Session ID: 0 Serial number: 0


SQL>       ALTER DATABASE DROP LOGFILE GROUP 1
*
ERROR at line 1:
ORA-01034: ORACLE not available
Process ID: 0
Session ID: 0 Serial number: 0


SQL>       ALTER DATABASE DROP LOGFILE GROUP 2
*
ERROR at line 1:
ORA-01034: ORACLE not available
Process ID: 0
Session ID: 0 Serial number: 0


SQL> SQL>       ALTER SYSTEM SET db_recovery_file_dest=''
*
ERROR at line 1:
ORA-01034: ORACLE not available
Process ID: 0
Session ID: 0 Serial number: 0


SQL> Disconnected
mv: cannot stat '/u01/app/oracle/product/11.2.0/xe/dbs/spfilebomb0069.ora': No such file or directory
mv: cannot stat '/u01/app/oracle/product/11.2.0/xe/dbs/orapwbomb0069': No such file or directory
#########################
DATABASE IS READY TO USE!
#########################
The following output is now a tail of the alert.log:
QMNC started with pid=24, OS id=766 
Completed: ALTER DATABASE OPEN
Tue Apr 13 08:02:48 2021
db_recovery_file_dest_size of 10240 MB is 0.98% used. This is a
user-specified limit on the amount of space that will be used by this
database for recovery-related files, and does not reflect the amount of
space available in the underlying filesystem or ASM diskgroup.
Starting background process CJQ0
Tue Apr 13 08:02:48 2021
CJQ0 started with pid=27, OS id=780
```

### Work

```sh
docker run --name oracle-11c \
-p 1521:1521 -p 5500:5500 \
-v /Users/lab/database/oracle/oradata:/opt/oracle/oradata \
--shm-size=1g  oracle/database:11.2.0.2-xe
```

```sh
ORACLE PASSWORD FOR SYS AND SYSTEM: 277b9cfafcdf5751

Oracle Database 11g Express Edition Configuration
-------------------------------------------------
This will configure on-boot properties of Oracle Database 11g Express 
Edition.  The following questions will determine whether the database should 
be starting upon system boot, the ports it will use, and the passwords that 
will be used for database accounts.  Press <Enter> to accept the defaults. 
Ctrl-C will abort.

Specify the HTTP port that will be used for Oracle Application Express [8080]:
Specify a port that will be used for the database listener [1521]:
Specify a password to be used for database accounts.  Note that the same
password will be used for SYS and SYSTEM.  Oracle recommends the use of 
different passwords for each database account.  This can be done after 
initial configuration:
Confirm the password:

Do you want Oracle Database 11g Express Edition to be started on boot (y/n) [y]:
Starting Oracle Net Listener...Done
Configuring database...Done
Starting Oracle Database 11g Express Edition instance...Done
Installation completed successfully.

SQL*Plus: Release 11.2.0.2.0 Production on Tue Apr 13 09:36:01 2021

Copyright (c) 1982, 2011, Oracle.  All rights reserved.


Connected to:
Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production

SQL> 
PL/SQL procedure successfully completed.

SQL> SQL> 
Database altered.

SQL> 
Database altered.

SQL> 
Database altered.

SQL> 
System altered.

SQL> 
System altered.

SQL> 
System altered.

SQL> 
Database altered.

SQL> 
Database altered.

SQL> SQL> 
System altered.

SQL> Disconnected from Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production
#########################
DATABASE IS READY TO USE!
#########################
The following output is now a tail of the alert.log:
  Current log# 5 seq# 4 mem# 0: /u01/app/oracle/oradata/XE/redo05.log
      ALTER DATABASE DROP LOGFILE GROUP 1
Deleted Oracle managed file /u01/app/oracle/fast_recovery_area/XE/onlinelog/o1_mf_1_j7bsgs75_.log
Completed:       ALTER DATABASE DROP LOGFILE GROUP 1
      ALTER DATABASE DROP LOGFILE GROUP 2
Deleted Oracle managed file /u01/app/oracle/fast_recovery_area/XE/onlinelog/o1_mf_2_j7bsgsfd_.log
Completed:       ALTER DATABASE DROP LOGFILE GROUP 2
Cleared LOG_ARCHIVE_DEST_1 parameter default value
Using LOG_ARCHIVE_DEST_1 parameter default value as /u01/app/oracle/product/11.2.0/xe/dbs/arch
ALTER SYSTEM SET db_recovery_file_dest='' SCOPE=BOTH;
```

## Connect to SQLPLus with sysdba

```sh
su -p oracle -c "sqlplus / as sysdba"
```

```sh
SQL*Plus: Release 11.2.0.2.0 Production on Tue Apr 13 09:39:56 2021

Copyright (c) 1982, 2011, Oracle.  All rights reserved.


Connected to:
Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production
```

## Install SQLPlus in MacOS

```sh
https://www.oracle.com/database/technologies/instant-client/macos-intel-x86-downloads.html#ic_osx_inst
```

## Connect to Oracle

```sh
sqlplus user/pass@(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(Host=hostname.network)(Port=1521))(CONNECT_DATA=(SID=remote_SID)))

sqlplus 'system/277b9cfafcdf5751@(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(Host=localhost)(Port=1521))(CONNECT_DATA=(SID=XE)))'

```
