# 1. Provide steps to create a directory inside a directory where the parent directory does not exist.

- To create a directory inside a directory where the parent directory does not exist, you can use the -p option with the mkdir command. For example:
  ```
  mkdir -p /path/to/parent/directory/new_directory


# 2. How to install a package on a Linux server when there is no internet connection?

- To install a package on a Linux server when there is no internet connection, you can download the package and its dependencies on a machine with internet access, transfer them to the offline server, and then use the `dpkg` or `rpm` package manager to install them.

# 3. How to access specific folders of Server A from Server B and Server C?

- To access specific folders of Server A from Server B and Server C, you can use `SSH` and `SCP` (Secure Copy Protocol). Use the `ssh` command to connect to Server A and `scp` to copy files and directories.

# 4. How to check all the running processes from a server?

- To check all running processes on a Linux server, you can use the ps command. For example, to list all processes, you can use:
  ```
  ps aux


# 5. Provide the command to delete all the files older than X days inside a specific directory.

- To delete all files older than X days inside a specific directory, you can use the find command along with the rm command. Here's an example:
```
find /path/to/directory -type f -mtime +X -exec rm {} \;
```





# 6. Create a shell script to identify the process ID
## A. script should as a user input for process ID<br>
 - You can create a shell script that prompts the user to enter a process ID and stores it in a variable. Here's a sample script:
  
  ```
  #!/bin/bash
  # Prompt the user to enter a process ID
  read -p "Enter the process ID: " pid

  # Print the entered process ID
  echo "You entered the process ID: $pid"
  ```
 - This script uses the read command to read user input and stores it in the pid variable. It then prints the entered process ID.
  
## B. If the process exists script should print the process ID and exit <br>
 - To check if a process with the given process ID exists, you can use the ps command. Here's an extended version of the script:
    ```
    #!/bin/bash

    # Prompt the user to enter a process ID
    read -p "Enter the process ID: " pid

    # Check if the process exists
    if ps -p $pid > /dev/null
    then
      echo "Process $pid exists."
    else
      echo "Process $pid doesn't exist."
    fi
 - In this script, it first prompts the user for the process ID, then uses the ps command to check if a process with the given ID exists. If it does, it prints a message indicating that the process exists; otherwise, it prints a message indicating that the process doesn't exist.<br>


## c. If the process doesn't exist script should print the process doesn't exist and asks for another input<br>
- Handle the case when the process doesn't exist and ask for another input.
- To handle the case when the process doesn't exist and ask for another input, you can use a loop.
   ```
   #!/bin/bash

   while true; do
    # Prompt the user to enter a process ID
    read -p "Enter the process ID (or 'q' to quit): " input

    # Check if the user wants to quit
    if [ "$input" == "q" ]; then
        echo "Exiting the script."
        break
    fi

    # Check if the process exists
    if ps -p $input > /dev/null; then
        echo "Process $input exists."
    else
        echo "Process $input doesn't exist."
    fi
    done
- This script continually prompts the user for a process ID, checks if it exists, and provides the option to quit by entering 'q'. If the process doesn't exist, it informs the user and continues the loop for another input.<br>

