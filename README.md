# Create_User_Linux

This repo was intended to create a bash script to create/manage new user and their group as instructed by the task 1 of the HNG intership 11th Cohort, you can join for free through this link https://hng.tech/internship or https://hng.tech/hire.

To automate user and group management with a Linux bash script I broke the task into this chunk and follow these steps

First, read the input file.
Examine the text file that contains the group names and usernames of the employees. Process each line, which is formatted as user;groups.

Step 2: Divide each line into the username and the groups that belong with it. For proper processing, eliminate any whitespace.

Step 3: Create Users and Groups
Ensure each user has a personal group with the same name as their username, then Create additional groups if they do not already exist, add the user to the specified groups.

Step 4: Set Up Home Directories
Each user should have a home directory, which should have the proper ownership and permissions configured. Passwords should also be generated and stored.

Step 5: Generate random passwords for each user.
The passwords should be safely kept in /var/secure/user_passwords.txt. 

Step 6: Log Actions
Record every action that is taken and save it in /var/log/user_management.log (e.g., user creation, group assignment, user already exists, etc.).

Step 7: Managing Errors
Set up error handling for overseeing cases like existing users or groups.

