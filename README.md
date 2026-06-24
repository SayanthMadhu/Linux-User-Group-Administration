## Linux User Administration 
Not Use sudo before each command if you are not logged in as the root user.

## 🔹 Step 1: Create a Group

Command:

sudo groupadd engineers

Explanation:-
. groupadd creates a new group.
. engineers is the name of the group.
  
Verify:
grep engineers /etc/group

Explanation:-
.  /etc/group stores all group information.
.  grep engineers searches for the newly created group.

OUTPUT:
<img width="407" height="77" alt="image" src="https://github.com/user-attachments/assets/c8c323cf-654b-49c8-8e66-030e7f939551" />

## 🔹 Step 2: Create User devuser1
Requirement:-

. Username: devuser1
. Default shell: /bin/bash
. Comment: Developer One
. Member of the engineers group
Command:
sudo useradd -m -s /bin/bash -c "Developer One" -G engineers devuser1

Explanation:-

.  useradd → Creates a new user.
. -m → Creates the user's home directory (/home/devuser1).
. -s /bin/bash → Sets Bash as the default login shell.
. -c "Developer One" → Adds a comment (usually the user's full name or description).
. -G engineers → Adds the user to the supplementary group engineers.
. devuser1 → Username

Verify:

grep devuser1 /etc/passwd

OUTPUT :
<img width="717" height="65" alt="image" src="https://github.com/user-attachments/assets/9b2f7c61-d0a9-4369-9169-b231e6d02ac0" />

Explanation:-

. /etc/passwd stores user account information.
. This confirms that the user was created successfully.

## 🔹 Step 3: Create User devuser2
Requirements:- 

. Username: devuser2
. Home directory: /customhome/devuser2
. Member of the engineers group

 Commant to Create the parent directory:-
 
 sudo mkdir -p /customhome

Explanation:-

. mkdir creates directories.
. -p creates the directory only if it does not already exist.

Commant to  Create the user:-

. sudo useradd -m -d /customhome/devuser2 -G engineers devuser2

Explanation:- 

. -m → Creates the home directory.
. -d /customhome/devuser2 → Sets a custom home directory.
. -G engineers → Adds the user to the engineers group.

Verify:- 

grep devuser2 /etc/passwd

OUTPUT:
<img width="672" height="65" alt="image" src="https://github.com/user-attachments/assets/b9ed6dab-4443-49c4-af9f-57d42f5ea752" />


## 🔹 Step 4: Set Passwords

Commant to Set password for devuser1:-

 sudo passwd devuser1
 
Explanation:-
. passwd sets or changes a user's password.
. You'll be prompted to enter the password twice.
Example:

. New password:
. Retype new password:

OUTPUT: 
<img width="337" height="77" alt="image" src="https://github.com/user-attachments/assets/b9fe6778-6d10-4dd6-babd-8453a8f6c913" />


Set password for devuser2
sudo passwd devuser2
Repeat the same process.

OUTPUT:
<img width="337" height="77" alt="image" src="https://github.com/user-attachments/assets/9a8e9ea5-b831-4f4a-812a-dc99f227bb00" />


## 🔹 Step 5: Configure Password Aging for devuser1

Requirements:-
. Minimum days: 2
. Maximum days: 45
. Warning days: 7

Command :- 

sudo chage -m 2 -M 45 -W 7 devuser1

Explanation:-
. chage changes password aging information.
. -m 2 → User must wait at least 2 days before changing the password again.
. -M 45 → Password expires after 45 days.
. -W 7 → User receives a warning 7 days before password expiration.
Verify:-

chage -l devuser1
Explanation

-l lists the password aging settings.
OUTPUT:-
<img width="692" height="147" alt="image" src="https://github.com/user-attachments/assets/0f3c6548-1453-4c0d-bb47-3c1c92e81ffd" />

## 🔹 Step 6: Give devuser1 Administrative (Sudo) Access

Ubuntu/Debian 

sudo usermod -aG sudo devuser1

RHEL/CentOS/Rocky

sudo usermod -aG sudo devuser1

Explanation:

usermod modifies an existing user.
. -a means append (don't remove existing groups).
. -G specifies supplementary groups.
Adding the user to the sudo (or wheel) group grants administrative privileges.

Verify:

groups devuser1

OUTPUT :
<img width="302" height="45" alt="image" src="https://github.com/user-attachments/assets/e9fa2985-72a5-491a-ae76-a1bd10a8da90" />

## 🔹 Step 7: Set an Expiry Date for devuser2

Command:- 

sudo chage -E $(date -d "+15 days" +%F) devuser2

Explanation:-

. date -d "+15 days" calculates the date 15 days from today.
. +%F formats the date as YYYY-MM-DD.
. chage -E sets the account expiration date.

Verify:

chage -l devuser2

OUTPUT:
<img width="617" height="142" alt="image" src="https://github.com/user-attachments/assets/5a4b34ae-b53e-46d8-86dc-d252d42814ef" />

## 🔹 Step 8: Lock devuser2

Command:-

sudo passwd -l devuser2

Explanation:-

. -l locks the user's password.
. The user cannot log in until the account is unlocked.
Verify

sudo passwd -S devuser2

OUTPUT: 

<img width="317" height="42" alt="image" src="https://github.com/user-attachments/assets/836e5460-aa6a-43c2-93a4-cb49d8b9eff8" />


L means Locked.

## 🔹 Step 9: Verification

commant to Verify user accounts

cat /etc/passwd | grep devuser

OUTPUT:
<img width="507" height="61" alt="image" src="https://github.com/user-attachments/assets/3c963d59-f5a9-4d56-87c1-268473656c93" />


Shows both user accounts if they exist.


Verify group membership

groups devuser1

Explanation

Displays all groups to which devuser1 belongs.

OUTPUT:
<img width="292" height="46" alt="image" src="https://github.com/user-attachments/assets/c23ef9bc-a8a2-4e2c-b34e-2bc563bc282f" />

Verify password policy

chage -l devuser1
Explanation

Displays password aging information for devuser1.
OUTPUT:
<img width="606" height="142" alt="image" src="https://github.com/user-attachments/assets/9fc18235-c51e-4722-97f6-7fd835b85862" />


Verify sudo privileges
sudo -l -U devuser1
Explanation

Lists the commands that devuser1 is allowed to run using sudo.
OUTPUT :
<img width="522" height="65" alt="image" src="https://github.com/user-attachments/assets/ddee4749-513f-49e9-8a3a-870e11444ca9" />
 -id
Verify custom home directory

ls -ld /customhome/devuser2
Explanation

ls lists files/directories.
-l shows detailed information.
-d displays the directory itself instead of its contents.

Output:
<img width="607" height="62" alt="image" src="https://github.com/user-attachments/assets/1c17efdb-0830-4151-8793-ca6618f5b60e" />

## 🔹 Step 10: Unlock devuser2

Command

sudo passwd -u devuser2

Explanation

-u unlocks the user account.

The user can log in again.

OUTPUT: 

<img width="607" height="62" alt="image" src="https://github.com/user-attachments/assets/9a8eede3-29fa-4016-a1bc-ae8eebac3789" />

## 🔹 Step 11: Delete Both Users

Delete devuser1
sudo userdel -r devuser1
Delete devuser2
sudo userdel -r devuser2
Explanation
userdel removes a user account.
-r also removes the user's home directory and mail spool.
OUTPUT:
<img width="572" height="102" alt="image" src="https://github.com/user-attachments/assets/76652c04-52ad-455e-8a50-c1713892f7cb" />

🔹 Step 12: Delete the Group
Command:-

sudo groupdel engineers

Explanation:

. groupdel deletes an existing group.
. The group must not be the primary group of any existing user.
. Commands Required for Submission (Run Before Cleanup)

cat /etc/passwd | grep devuser

groups devuser1

chage -l devuser1

sudo -l -U devuser1

ls -ld /customhome/devuser2
These commands verify:
cat /etc/passwd | grep devuser → Both users were created.
groups devuser1 → devuser1 belongs to the engineers and sudo (or wheel) groups.
chage -l devuser1 → Password aging policy is configured correctly.
sudo -l -U devuser1 → devuser1 has administrative privileges.
ls -ld /customhome/devuser2 → The custom home directory for devuser2 exists.

OUTPUT: 
<img width="582" height="207" alt="image" src="https://github.com/user-attachments/assets/c4c650fc-60fe-4f45-b93d-8683dd79fc3c" />

