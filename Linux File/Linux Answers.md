# 1. Provide steps to create a directory inside a directory where the parent directory does not exist.

- #### Navigate to the desired location where you want to create the directory
    ```
      cd /path/to/parent/directory
    ```
- #### Create the directory along with its parent directories using the -p option
    ```
      mkdir -p non_existing_parent_directory/new_directory
    ```

- #### Verify the creation of the directory
    ```
  ls non_existing_parent_directory

    ```


# 2. How to install a package on a Linux server when there is no internet connection?

- #### Navigate to the Package Directory: Change your working directory to the location where you transferred the package.
    ```
      cd /path/on/target_server
    ```

- #### Install the Package: Install the package using the package manager
     ```
      sudo dpkg -i package_name
     ```
-  #### Resolve Dependency Issues:  If there are unmet dependencies, you might need to install them manually in a similar manner.
      ```
       sudo apt-get -f install    # For Debian-based systems
      ```





# 3. How to access specific folders of Server A from Server B and Server C?

- To access specific folders on Server A from Server B and Server C, you can use the Secure Shell (SSH) File System (SSHFS) to mount the remote directories as if they were local. Here are the detailed steps:

### On Server A:

1. **Install SSH Server (if not installed):**
   Ensure that an SSH server is installed on Server A. If not, you can install it based on your Linux distribution. For example, on Debian-based systems:

   ```bash
   sudo apt-get update
   sudo apt-get install openssh-server
   ```

2. **Enable SSH Service:**
   Start or enable the SSH service on Server A:

   ```bash
   sudo service ssh start    # For systemd systems
   ```

   Or for systems using systemd:

   ```bash
   sudo systemctl start ssh
   ```

### On Server B and Server C:

3. **Install SSHFS (if not installed):**
   Make sure SSHFS is installed on Server B and Server C. Install it using your package manager. For example, on Debian-based systems:

   ```bash
   sudo apt-get update
   sudo apt-get install sshfs
   ```

4. **Create Local Mount Points:**
   Choose local directories on Server B and Server C where you want to mount the remote folders. For example:

   ```bash
   mkdir /path/on/B/mount_point
   mkdir /path/on/C/mount_point
   ```

### Mounting Remote Directory on Server B:

5. **Mount Server A on Server B:**
   Use SSHFS to mount the remote directory from Server A onto Server B:

   ```bash
   sshfs user@ServerA:/path/on/A /path/on/B/mount_point
   ```

   Replace `user` with your username on Server A, and adjust the paths accordingly.

### Mounting Remote Directory on Server C:

6. **Mount Server A on Server C:**
   Similarly, mount the remote directory from Server A onto Server C:

   ```bash
   sshfs user@ServerA:/path/on/A /path/on/C/mount_point
   ```

   Replace `user` with your username on Server A, and adjust the paths accordingly.

### Unmounting:

7. **Unmounting (when finished):**
   When you're done accessing the remote directories, unmount them:

   ```bash
   fusermount -u /path/on/B/mount_point    # on Server B
   fusermount -u /path/on/C/mount_point    # on Server C
   ```

These steps will allow you to access specific folders on Server A from Server B and Server C as if they were local directories. Adjust the paths and user information as per your system configuration.




















# 4. How to check all the running processes from a server?

- To check all the running processes on a server, you can use the `ps` command. Here are the detailed steps:

### Method 1: Basic `ps` Command

1. **List All Processes:**
   Use the following command to list all the running processes:

   ```bash
   ps aux
   ```

   This command provides a detailed list of all processes, including information about the user, process ID (PID), CPU usage, memory usage, command, and more.

### Method 2: `top` Command

2. **Interactive Process Viewer (Optional):**
   Alternatively, you can use the `top` command, which provides an interactive view of the processes along with real-time updates:

   ```bash
   top
   ```

   Use the arrow keys to navigate, and press `q` to exit.

### Method 3: `htop` Command (if installed)

3. **Interactive Process Viewer (Optional, if installed):**
   If you have `htop` installed, you can use it for a more user-friendly and interactive process view:

   ```bash
   htop
   ```

   Similar to `top`, use arrow keys to navigate, and press `q` to exit.

### Method 4: `pgrep` and `pkill` Commands

4. **Find a Specific Process by Name (Optional):**
   If you are looking for a specific process by name, you can use the `pgrep` command. For example, to find the process ID of a process named "example":

   ```bash
   pgrep -f "example"
   ```

   If you want to kill a process by name, you can use the `pkill` command. Be cautious when using `pkill` as it will terminate all processes matching the specified name.

   ```bash
   pkill -f "example"
   ```

These commands provide various ways to view and manage running processes on a server. Choose the method that best suits your needs. Adjustments may be necessary based on the specific Linux distribution you are using.






# 5. Provide the command to delete all the files older than X days inside a specific directory.

- To delete all files older than X days inside a specific directory, you can use the `find` command along with the `-mtime` option. Here's the command:

```bash
find /path/to/directory -type f -mtime +X -exec rm {} \;
```

Replace `/path/to/directory` with the actual path of the directory where you want to delete files, and replace X with the number of days. This command will find all files (`-type f`) in the specified directory that are older than X days (`-mtime +X`) and execute the `rm` (remove) command on each of them.

Explanation of the command options:

- `/path/to/directory`: The path to the directory where you want to delete files.
- `-type f`: Specifies that only files (not directories or other types) should be considered.
- `-mtime +X`: Specifies files with a modification time older than X days.
- `-exec rm {} \;`: Executes the `rm` command on each found file. The `{}` is a placeholder for the file name, and `\;` indicates the end of the `-exec` command.

Be cautious when using this command, as it permanently deletes files. Test it on a small set of files or use the `-print` option with `find` before using the `rm` command to see a list of files that match the criteria.





# 6. Create a shell script to identify the process ID
- Here's a simple shell script:

```bash
#!/bin/bash

# Prompt user for process ID
read -p "Enter process ID: " pid

# Check if the process exists
if ps -p "$pid" > /dev/null; then
    echo "Process $pid exists."
    exit 0
else
    echo "Process $pid does not exist."
    # Ask for another input
    read -p "Do you want to check another process? (y/n): " choice
    if [ "$choice" == "y" ]; then
        # Recursively call the script
        exec "$0"
    else
        echo "Exiting script."
        exit 1
    fi
fi
```

Save this script to a file, for example, `check_process.sh`, and make it executable:

```bash
chmod +x check_process.sh
```

Then you can run the script:

```bash
./check_process.sh
```

This script prompts the user for a process ID, checks if the process exists, and provides options to check another process or exit the script. Adjustments can be made based on your specific requirements or preferences.
