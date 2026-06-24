## Linux User Administration – End-to-End Practical Lab (With Explanation)
Note: Use sudo before each command if you are not logged in as the root user.

🔹 Step 1: Create a Group
Command
sudo groupadd engineers
Explanation
groupadd creates a new group.
engineers is the name of the group.
Verify
grep engineers /etc/group
Explanation:

/etc/group stores all group information.
grep engineers searches for the newly created group.
Expected output:

engineers:x:1001:
🔹 Step 2: Create User devuser1
Requirements
Username: devuser1
Default shell: /bin/bash
Comment: Developer One
Member of the engineers group
Command
sudo useradd -m -s /bin/bash -c "Developer One" -G engineers devuser1
Explanation
useradd → Creates a new user.
-m → Creates the user's home directory (/home/devuser1).
-s /bin/bash → Sets Bash as the default login shell.
-c "Developer One" → Adds a comment (usually the user's full name or description).
-G engineers → Adds the user to the supplementary group engineers.
devuser1 → Username.
Verify
grep devuser1 /etc/passwd
Explanation

/etc/passwd stores user account information.
This confirms that the user was created successfully.
Example output:

devuser1:x:1001:1002:Developer One:/home/devuser1:/bin/bash
🔹 Step 3: Create User devuser2
Requirements
Username: devuser2
Home directory: /customhome/devuser2
Member of the engineers group
Create the parent directory
sudo mkdir -p /customhome
Explanation
mkdir creates directories.
-p creates the directory only if it does not already exist.
Create the user
sudo useradd -m -d /customhome/devuser2 -G engineers devuser2
Explanation
-m → Creates the home directory.
-d /customhome/devuser2 → Sets a custom home directory.
-G engineers → Adds the user to the engineers group.
Verify
grep devuser2 /etc/passwd
Example output:

devuser2:x:1002:1003::/customhome/devuser2:/bin/sh
🔹 Step 4: Set Passwords
Set password for devuser1
sudo passwd devuser1
Explanation
passwd sets or changes a user's password.
You'll be prompted to enter the password twice.
Example:

New password:
Retype new password:
passwd: password updated successfully
Set password for devuser2
sudo passwd devuser2
Repeat the same process.

🔹 Step 5: Configure Password Aging for devuser1
Requirements
Minimum days: 2
Maximum days: 45
Warning days: 7
Command
sudo chage -m 2 -M 45 -W 7 devuser1
Explanation
chage changes password aging information.
-m 2 → User must wait at least 2 days before changing the password again.
-M 45 → Password expires after 45 days.
-W 7 → User receives a warning 7 days before password expiration.
Verify
chage -l devuser1
Explanation

-l lists the password aging settings.
Example:

Minimum number of days between password change : 2
Maximum number of days between password change : 45
Number of days of warning before password expires : 7
🔹 Step 6: Give devuser1 Administrative (Sudo) Access
Ubuntu/Debian
sudo usermod -aG sudo devuser1
RHEL/CentOS/Rocky
sudo usermod -aG wheel devuser1
Explanation
usermod modifies an existing user.
-a means append (don't remove existing groups).
-G specifies supplementary groups.
Adding the user to the sudo (or wheel) group grants administrative privileges.
Verify
groups devuser1
Example:

devuser1 : devuser1 engineers sudo
🔹 Step 7: Set an Expiry Date for devuser2
Command
sudo chage -E $(date -d "+15 days" +%F) devuser2
Explanation
date -d "+15 days" calculates the date 15 days from today.
+%F formats the date as YYYY-MM-DD.
chage -E sets the account expiration date.
Verify
chage -l devuser2
Look for:

Account expires : Jul 09, 2026
🔹 Step 8: Lock devuser2
Command
sudo passwd -l devuser2
Explanation
-l locks the user's password.
The user cannot log in until the account is unlocked.
Verify
sudo passwd -S devuser2
Example:

devuser2 L 2026-06-24 0 99999 7 -1
L means Locked.

🔹 Step 9: Verification
Verify user accounts
cat /etc/passwd | grep devuser
Explanation

Shows both user accounts if they exist.
Example:

devuser1:x:1001:1002:Developer One:/home/devuser1:/bin/bash
devuser2:x:1002:1003::/customhome/devuser2:/bin/sh
Verify group membership
groups devuser1
Explanation

Displays all groups to which devuser1 belongs.
Example:

devuser1 : devuser1 engineers sudo
Verify password policy
chage -l devuser1
Explanation

Displays password aging information for devuser1.
Verify sudo privileges
sudo -l -U devuser1
Explanation

Lists the commands that devuser1 is allowed to run using sudo.
Example:

User devuser1 may run the following commands:
    (ALL : ALL) ALL
Verify custom home directory
ls -ld /customhome/devuser2
Explanation

ls lists files/directories.
-l shows detailed information.
-d displays the directory itself instead of its contents.
Example:

drwx------ 2 devuser2 devuser2 4096 Jun 24 12:00 /customhome/devuser2
🔹 Step 10: Unlock devuser2
Command
sudo passwd -u devuser2
Explanation
-u unlocks the user account.
The user can log in again.
🔹 Step 11: Delete Both Users
Delete devuser1
sudo userdel -r devuser1
Delete devuser2
sudo userdel -r devuser2
Explanation
userdel removes a user account.
-r also removes the user's home directory and mail spool.
🔹 Step 12: Delete the Group
Command
sudo groupdel engineers
Explanation
groupdel deletes an existing group.
The group must not be the primary group of any existing user.
📋 Commands Required for Submission (Run Before Cleanup)
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
