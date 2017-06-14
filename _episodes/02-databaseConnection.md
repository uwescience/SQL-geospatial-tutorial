---
title: "Database Connection"
teaching: 5
exercises: 10
questions:
- "How do I connect to a cloud-based relational database?"
objectives:
- "Learn how to connect to a relational database using a graphical user interface called dBeaver"
keypoints:
- "dBeaver provides a simple interface for viewing database tables and testing SQL queries"
---

## Step 1: Connecting to the DSSG tutorial database

#### Download software
We will use the [DBeaver](http://dbeaver.jkiss.org/download/) software to connect to the database.

* Windows - [click here](http://dbeaver.jkiss.org/files/dbeaver-ce-latest-x86_64-setup.exe) and download the file to install the application

* Mac OSX - [click here](http://dbeaver.jkiss.org/files/dbeaver-ce-latest-macos.dmg) and download the file to install the application

* Linux - [click here](http://dbeaver.jkiss.org/files/dbeaver-ce_latest_amd64.deb) and download the file to install the application

* once you have the software installed, start DBeaver

#### Connect to the database

1. Click on the database icon:
<br><br>
<img src="../assets/img/connection/toolbar.png" width = "400" border = "10">
<br><br><br><br>
2. Search `postgresql` as connection type.
<br><br>
<img src="../assets/img/connection/searchpostgres.PNG" width="400">
<br><br><br><br>
3. Fill in the connection information.

- The host name is: someurl.amazonaws.com
- The port is: 5432
- The username is: some_username
- The password is: somepassword
<br><br>
<img src="../assets/img/connection/connection.PNG" width="400">
<br><br><br><br>
4. Click `Test Connection ...` to ensure that you have entered the correct credentials.
<br><br>
5. You might get prompted with the download driver files window if this is your first time using postgresql.
Click on Download and follow the instructions.
<br><br>
<img src="../assets/img/connection/driverinstall.PNG" width="400">
<br><br><br><br>
6. If everything is connected you will get a success message.
<br><br>
<img src="../assets/img/connection/serversuccess.PNG" width="400">
<br><br><br><br>
7. Keep pressing next on the connection window, until finish. Once you are through, you will see the following when expanded.
We only need to access the tables. Double click on `seattlecrimeincidents` table, then to `Data` tab so see the data.
Take some time to become familiar with the contents of the table.
<br><br>
<img src="../assets/img/connection/viewdata.PNG" width = "600" border = "1">
<br><br><br><br>
