<h1>SQL problems & solutions</h1>

<h4>#1. Tackling Error:</h4></br></br>

-> Faced issue while building the grail project using gradle where the following error is encountered: </br>
Your Firefox profile cannot be loaded. It may be missing or inaccessible. </br>
# It seems the the error was due to firefox being installed at default with snap. So the solution is to install firefox without snap. </br>
# One such problem faced with selenium test suit with firefox driver is here : https://stackoverflow.com/questions/72405117/selenium-geckodriver-profile-missing-your-firefox-profile-cannot-be-loaded </br>
# Hence use the debian apprach using the following guideline : https://www.omgubuntu.co.uk/2022/04/how-to-install-firefox-deb-apt-ubuntu-22-04 </br>
***NOTE: IT MAY TAKE SOME TIME TO REMOVE FIREFOX FROM SNAP. ALSO THE REPOSITORY MIGHT NOT BE GONE FROM SNAP; SO LET IT STAY THEIR AFTER REMOVAL.*** </br>



</br></br></br>


<h4>#2. PostgreSQL </h4> </br>
After downloading the postgresql in linux through terminal the default user being created is "postgres" without any password </br>
So to create password we can go to the the shell of postgres user by using the following process initially</br>

```
sudo -i -u postgres
```

which will ask for the system password. After providing the password, this will give rise to a shell which is of the user postgres. There type the following command:

```
psql
```

this will provide postgresql shell where we can create new roles / users initially following the official document : https://www.postgresql.org/docs/14/role-attributes.html </br>

Moreover, here we can also change or add password to any user using the following commands:

```
\password <username>
```

then it will give prompt to provide the passwords (we can use the same process to add passwords to the initially generated user 'postgres' or any other users created later manually). </br>
However, this might not be directly usable since giving the following command in normal terminal : 

```
psql <database_name> -U username
```

which will give rise to the following error : 
```
psql: error: connection to server on socket "/var/run/postgresql/.s.PGSQL.5432" failed: FATAL:  Peer authentication failed for user "username"
```

This will be mostly same for any user that is created if in the file 'pg_hba.conf' the METHOD for both users 'postgres' or any local user (referred to as 'all') is given 'peer'. </br>
So we need to change the peer method to md5 by opening the file with any editor with sudo or root permission and then restart the postgresql server using the following commands:

```
 sudo systemctl restart postgresql
```

This is done so that the new changes to the file can be loaded and implemented. </br>
Now using the command : 
```
psql <database_name> -U <username>
```

will ask for that users prompt without giving peer authentication failour but it will give rise to the following error if the database was not created earlier:

```
psql: error: connection to server on socket "/var/run/postgresql/.s.PGSQL.5432" failed: FATAL:  database "postgresProject1" does not exist
```

The error is normal ; we can create the database using the following command: 

```
createdb <database_name> -U <usernae>
```
# NOTE : if the user was not given permission to create database then the following error will be shown:

```
createdb: error: database creation failed: ERROR:  permission denied to create database
```

 		
 We can also access the postgre sql shell using the following command now if the user is created and is allowed to login :
 
 ```
 psql <database_name> -U <username>
 ```
 
 This will select the provided database shell if already created. </br></br>
 		
 		*** i have saved the password for postgres the same as my system password ***</br>
 		*** rolecreator with rolecreator ***</br>
 		*** dbcreator with dbcreator ***</br>
 		*** user1 with us4r1 ***</br>
#NOTE : IT IS NOT MENDATORY TO DO SO BUT IN MY CASE SINCE I WAS PRACTICING WITH MULTIPLE USER, IT WAS EASIER TO KEEP TRACK OF!!</br>
 		
 		
However, if the the user or role being created was not given permission to login then after using the command:
```
psql <database_name> -U <username>
```
it will ask for the password. And on providing the password it will give the following error:
```
psql: error: connection to server on socket "/var/run/postgresql/.s.PGSQL.5432" failed: FATAL:  role "dbcreator" is not permitted to log in
```
 
While trying to create user if the user trying to create another user does not have permission to create role then the following error arises while running the following command:
```
createuser <new_username> -U <username>
 
createuser: error: creation of new role failed: ERROR:  permission denied to create role
```
But if the user has permission to create another user but do not have the permission to login then the following error arises:
```
createuser: error: connection to server on socket "/var/run/postgresql/.s.PGSQL.5432" failed: FATAL:  role "rolecreator" is not permitted to log in
```
 			
Login permission can be given to a user by the following command on the postgresql shell:
```
ALTER ROLE dbcreator  WITH LOGIN;
```
Other permissions can be given with similar statements. </br>
 		
The following command can be used at the postgresql shell to see the role names and their permissions:
```
\du
```
Also, a detailed view can be seen using the following command:
```
SELECT * FROM pg_roles;
```

 		
