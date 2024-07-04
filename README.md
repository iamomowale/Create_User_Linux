# Create_User_Linux

This repo was intended to create a bash script to create/manage new user and their group as instructed by the task 1 of the HNG intership 11th Cohort, you can join for free through this link https://hng.tech/internship or https://hng.tech/hire.

To automate user and group management with a Linux bash script I broke the task into this chunk and follow these steps

First, read the input file.
Examine the text file that contains the group names and usernames of the employees. Process each line, which is formatted as user;groups.
i.e ```script
filename="$1"
if [ ! -f "$filename" ]; then
echo "Users list file $filename not found."
exit 1
fi

while IFS=';' read -r user groups; do
    user=$(echo "$user" | xargs)
    groups=$(echo "$groups" | xargs | tr -d ' ')

    # Replace commas with spaces for usermod group format
    groups=$(echo "$groups" | tr ',' ' ')
    create_user "$user" "$groups"
done < "$filename"
```


Step 2: Divide each line into the username and the groups that belong with it. For proper processing, eliminate any whitespace.

Step 3: Create Users and Groups
Ensure each user has a personal group with the same name as their username, then Create additional groups if they do not already exist, add the user to the specified groups.
 i.e
```script
# Create personal group for the user
groupadd "$user"

# Create additional groups if they do not exist
IFS=' ' read -ra group_array <<< "$groups"

# Log the group array
log_action "User $user will be added to groups: ${group_array[*]}"

for group in "${group_array[@]}"; do
    group=$(echo "$group" | xargs)  # Trim whitespace
    if ! getent group "$group" &>/dev/null; then
        groupadd "$group"
        log_action "Group $group created."
    fi
done
```

Step 4: Set Up Home Directories
Each user should have a home directory, which should have the proper ownership and permissions configured. Passwords should also be generated and stored.
```script
# Create user with home directory and shell, primary group set to the personal group
useradd -m -s /bin/bash -g "$user" "$user"
if [ $? -eq 0 ]; then
    log_action "User $user created with primary group: $user"
else
    log_action "Failed to create user $user."
    return
fi
```

Step 5: Generate random passwords for each user.
The passwords should be safely kept in /var/secure/user_passwords.txt. 

```script
password=$(</dev/urandom tr -dc A-Za-z0-9 | head -c 12)
echo "$user:$password" | chpasswd
echo "$user,$password" >> "/var/secure/user_passwords.txt"
```

Step 6: Log Actions
Record every action that is taken and save it in /var/log/user_management.log (e.g., user creation, group assignment, user already exists, etc.).
```script
echo "$(date) - $1" >> "/var/log/user_management.log"
```

Step 7: Managing Errors
Set up error handling for overseeing cases like existing users or groups.

```script
if id "$user" &>/dev/null; then
        log_action "User $user already exists."
        return
    fi
```

