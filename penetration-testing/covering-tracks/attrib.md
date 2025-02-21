# 🪟 Attrib

Artifacts are the objects in a computer system that hold important information about the activities that are performed by user. Every operating system hides its artifacts such as internal task execution and critical system files.

***

## Hide a folder named **Test**

1. Open cmd and create a directory named **Test**
2. To hide the folder/file, type `attrib +h +s +r Test`
3. To unhide the folder/file, type `attrib -h -s -r Test`&#x20;

***

## Hide User Account in the Machine

1. Add User Account (Test) to the machine, by `net user Test /add`&#x20;
2. Active User Account (Test), by `net user Test /active:yes`&#x20;
3. Hide the user account, by `net user Test /active:no`&#x20;
