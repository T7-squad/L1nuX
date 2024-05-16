- *table* : organizes the information into a list.
- *record* : inside a table, a set of information about a table (First name,Last name,Address...).
- *fields* : 

- Example :
  ![[Pasted image 20240313130342.png]]

## Common SQL Statements 

![[Pasted image 20240313130427.png]]
![[Pasted image 20240313130445.png]]

## Configuring PostgreSQL

- Install the postgresql with `sudo apt install postgresql` .
- The installation creates a user called `postgres` within the `/etc/passwd` and `/etc/shadow`, that has the home directory of `/var/lib/postgresql` and the shell of `/bin/bash`.
  ![[Pasted image 20240313131014.png]]
  
- You need to assign a password to the user `postgres` by entering the password with `passwd postgres`.
  ![[Pasted image 20240313131048.png]]

- The postgresql config file is located in the `/etc/postgresql/15/main/postgresql.conf` for Ubuntu. 
  (You can find your config file otherwise by typing : `find / -type f -name "postgresql.conf" 2>/dev/null`).
  
## Configuring PostgreSQL databases

![[Pasted image 20240313131410.png]]

![[Pasted image 20240313140834.png]]


1. *Create a Database :*

Create a Database by entering the command `sudo -u postgres createdb new` new is the name of the database .

2. *Entering the Database :*

You need to run the command : `sudo postgres psql new` 
![[Pasted image 20240313134100.png]]

3. *Create a Table :* 

Lets create a Table that look like this :
![[Pasted image 20240313140221.png]]

- Naming the Table (employee):
  
```sql
sampledb=# CREATE TABLE employee (Name char(20), Dept char(20), Title char(20));
```

- Entering the Values of each row :
  
```sql
sampledb=# INSERT INTO employee VALUES ('Jeff Smith','Research','Analyst');

sampledb=# INSERT INTO employee VALUES ('Mary Wong','Accounting','Manager');

sampledb=# INSERT INTO employee VALUES ('Pat Clarke','Marketing','Coordinator');
```


- To delete a table, use the command :

```sql
DROP TABLE employee;
```

4. Display the Table :

To display the table use the command :

```sql
\d
```



> [!NOTE]
> - By entering the record of the Table, you need to keep in mind that the Information is entered horizontally ! 




