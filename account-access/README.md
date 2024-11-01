# Unauthorized account access

## Introduction
In this section, I will discuss accessing Supercell games accounts without Supercell ID.  
I consider this section to be somewhat experimental, but I will strive to make it as clear and readable as possible.  

Some elements presented here were discovered through the [request section](https://github.com/slayy2357/mimi/blob/main/requests/README.md)

Please note that all actions taken here are conducted using my own accounts. The purpose is to create documentation about potential exploits, not to engage in any malicious activities.

## Parts
- [Login](#login)
  - [Recording login sequence](#recording-login-sequence)
  - [Login without Supercell ID](#login-without-supercell-id)
- [Logged](#logged)
  - [Bugs when logged using method](#bugs-when-logged-using-method)
  - [Logged note](#logged-note)

## Login
This method involves connecting to an account and modifying the login information in real-time using the previously captured login sequence. It works for the game account but not for Supercell ID at the moment. This means that when you log in using this method, you will be connected to the game account through the modified login sequence, while also being connected to the Supercell ID account you would have logged into if you hadn't altered the login sequence.  

Keep in mind that this is only a 'temporary' access. Since we aren’t connected to the account’s Supercell ID, it won’t be saved, and you will always need to modify the login using the login sequence to access the account.  

Here is a very simplified example :
``` text
Basic Login:
select(supercell_ID_account) --> login(login_sequence) --> ...

Method:
select(supercell_ID_account) --> login(dumped_login_sequence) --> ... // the Supercell ID account doesn’t matter
```

### Recording login sequence

### Login without Supercell ID

## Logged

### Bugs when logged using method

In this example, I’m connected to the game account using this method, and you can see that it’s not the same as the Supercell ID account (but as I said in the login intro, this is not really a bug) :
![alt text](https://raw.githubusercontent.com/slayy2357/mimi/refs/heads/main/account-access/pictures/1.png)  

The friend list is synced to Supercell ID, so you will only see Supercell ID accounts friends (and you x2, due to the Supercell ID account) : 
![alt text](https://raw.githubusercontent.com/slayy2357/mimi/refs/heads/main/account-access/pictures/2.png)  

You will also always see yourself in the menu due to the Supercell ID account : 
![alt text](https://raw.githubusercontent.com/slayy2357/mimi/refs/heads/main/account-access/pictures/3.png)

### Logged note
Aside from the bugs related to the Supercell ID bypass, everything works as expected when you’re connected to the game account.