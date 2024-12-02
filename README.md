# Building Own Git Server
In this project, I set up my own Git server on an AWS EC2 instance, configured SSH keys, and learned how to create and manage repositories using the Git command line. I explored Git's internal workings, including objects and commit history, and also set up a web based Git UI for easier repository management. This hands on experience deepened my understanding of how Git functions and how to manage a custom Git server.

## What I Did
1. **Setup Git Server on AWS EC2:**
   - Created an EC2 instance on AWS and downloaded the PEM file for SSH access.
   - Installed Git on the EC2 instance using `sudo apt install git`.
   - Created a user (`git`) on the server for managing repositories.

2. **Created and Configured Git Repositories:**
   - Initialized a bare repository (`git init --bare`) in the `/var/lib/git/` directory.
   - Set up proper permissions and ownership for the Git repository folder.
   - Explored the internal structure of a Git repository, including how commits and changes are tracked.

3. **SSH Key Configuration:**
   - Generated an SSH key on the local machine using `ssh-keygen -t ed25519`.
   - Added the generated SSH public key to the `authorized_keys` file on the server for secure communication.
   - Successfully cloned the repository from the server to the local machine and made changes to test the setup.

4. **Commit Changes and Push to Server:**
   - Created an `index.html` file, committed changes, and pushed them to the Git server.
   - Observed the creation of objects in the Git repository, including commit IDs and corresponding object files.

5. **Explored Git Internals:**
   - Navigated through the Git objects directory (`/var/lib/git/my-review-app.git/objects`) to see how Git stores commit data (commit hashes, file changes).
   - Used the `git diff` command to compare two commit versions and track file changes over time.

6. **Used Git UI Tools:**
   - Installed and explored `git-web` for a simple web interface to interact with Git repositories.
   - Configured the server's security group to allow inbound traffic on port `1234` for web access.

7. **Worked with Multiple Projects:**
   - Created a second project repository on the server and repeated the process of cloning, committing, and pushing code.
   - Explored how multiple repositories can be managed on the same server.

8. **Git Collaboration Setup:**
   - Experimented with pushing and pulling code between the local machine and the server to simulate collaboration.
   - Added additional commits and tracked changes using `git log` and `git diff`.

## Steps

### 1. Configuring Git Server

1. **Create an AWS EC2 instance** and download the PEM file for SSH access.
   
2. **SSH into the machine**:
    ```bash
    ssh -i "git.pem" ubuntu@<your-ec2-ip-address>
    ```

3. **Install Git**:
    ```bash
    sudo apt install git
    ```

4. **Create a new user for Git**:
    ```bash
    sudo adduser git
    ```

5. **Set up the repository directory**:
    ```bash
    sudo chown -R git /var/lib/git
    sudo su git
    cd /var/lib/git
    mkdir my-review-app.git
    ```

6. **Initialize the repository**:
    ```bash
    git init --bare
    ```

### 2. Setting up SSH Key Authentication

1. **Generate SSH key on your local machine**:
    ```bash
    ssh-keygen -t ed25519 -C "git-user"
    cat ~/.ssh/id_ed25519.pub
    ```

2. **Copy the SSH key to the server**:
    ```bash
    ssh git@<your-ec2-ip-address>
    cd ~/.ssh/
    vim authorized_keys
    ```

    Paste the SSH key into the file.

3. **Clone the repository locally**:
    ```bash
    git clone git@<your-ec2-ip-address>:/var/lib/git/my-review-app.git
    ```

### 3. Pushing and Pulling Code

1. **Create a simple HTML file** (`index.html`) and add it to the repository.
   
2. **Commit and push the changes**:
    ```bash
    git add .
    git commit -m "first commit"
    git push
    ```

3. **Check the commit on the server**:
    Navigate to the objects directory to see the commit:
    ```bash
    cd /var/lib/git/my-review-app.git/objects
    ```

### 4. Exploring Git Objects

- After pushing the first commit, you can check the commit ID in the `objects` folder on the server.

- Similarly, for subsequent commits, Git stores data in objects. You can use the `git diff` command to see changes between commits:
    ```bash
    git diff <commit-id-1> <commit-id-2>
    ```

### 5. Installing Git UI (gitweb)

1. **Install Ruby and GitWeb**:
    ```bash
    sudo apt install ruby
    sudo apt install gitweb
    ```

2. **Start GitWeb**:
    ```bash
    cd /var/lib/git/my-review-app.git
    git instaweb --httpd=webrick
    ```

3. **Configure AWS security group** to allow access to port 1234.
   ![image](https://github.com/user-attachments/assets/15e1308f-88a3-42bf-8da7-1d7ba7b17b6f)


4. **Access the GitWeb UI** via the browser at:
    ```bash
    http://<your-ec2-ip-address>:1234
    ```
    ![image](https://github.com/user-attachments/assets/70d0c5f2-c4c9-4bcb-aee9-6e48b54fe64c)
    ![image](https://github.com/user-attachments/assets/e96020b2-e64a-4f99-839f-41185b0877d5)
    ![image](https://github.com/user-attachments/assets/557a8fcd-5ad8-4167-8b5b-7c27b787d3eb)




### 6. Changing Project Description

You can edit the project description by modifying the `description` file in the repository:
```bash
vim description
```
![image](https://github.com/user-attachments/assets/b970aaac-4936-48b0-94fc-f27979204baf)


### 7. Creating 3rd Commit

To create the 3rd commit, follow these commands:

```bash
# Add changes or create new files
$ git add .

# Commit the changes with a message
$ git commit -m "third commit"

# Push the changes to the remote repository
$ git push
```
![image](https://github.com/user-attachments/assets/8d5e3c8b-c041-4dd5-bead-255ac38aed9d)

## 8. Creating 2nd Project

To create a second project, follow these steps:

1. **Initialize a Bare Git Repository**:

Navigate to the directory where you want to create the new project, and initialize a bare Git repository:

```bash
$ cd /path/to/your/git/folder
$ git init --bare project-2.git
Initialized empty Git repository in /path/to/your/git/folder/project-2.git/
```

2. **Clone the New Repository Locally**:
   
Clone the newly created bare repository to your local machine to start working on it:

``` bash
$ git clone git@<your-ec2-ip>:<path-to-repo>/project-2.git
Cloning into 'project-2'...
warning: You appear to have cloned an empty repository.
done.
```

3. **Create Your Files and Commit Changes**:
   
Navigate to the cloned repository directory, create the necessary files, and then add, commit, and push your changes.

```bash
$ cd project-2
$ touch index.js    # Create a new JavaScript file
$ git add .         # Add the files to the staging area
$ git commit -m "Initial commit for project-2"  # Commit the changes
[master 123abc] Initial commit for project-2
 1 file changed, 1 insertion(+)

$ git push          # Push the changes to the remote repository

To git@<your-ec2-ip>:<path-to-repo>/project-2.git
   7770d0e..123abc  master -> master
```

4. **Verify the Changes** :
   
You can now verify that the files have been successfully committed and pushed to your new project repository.
```bash
$ git log
commit 123abc (HEAD -> master)
Author: Your Name <your.email@example.com>
Date:   Date

    Initial commit for project-2

```
![image](https://github.com/user-attachments/assets/47a24437-98a4-4003-9087-4476ab582886)
![image](https://github.com/user-attachments/assets/0e2c7599-98dc-45ea-9674-d67ac94c87cb)





## Key Learnings
- **Understanding Git Internals:** 
  - Git stores each commit as an object in its internal storage, which is organized by commit hash.
  - The structure of a Git repository includes various directories and files like `objects`, `refs`, and `config` that store data about commits, branches, and configurations.

- **SSH Key Integration:** 
  - SSH keys are essential for secure communication between the client and the server, eliminating the need for passwords.
  - Learned how to generate and add SSH keys to the server for authentication.

- **Git and Collaboration:** 
  - Git is a powerful tool for version control, and setting up a custom server helped understand the underlying process of committing changes and managing repositories.

- **Git Web Interface:** 
  - Installing tools like `gitweb` can provide a simple web interface to manage Git repositories, enhancing collaboration and ease of use.

## Final Thoughts
This project provided me with hands-on experience in Git's internal mechanics, server configuration, SSH key setup, and collaboration with a custom Git server. It enhanced my understanding of version control systems, both from a user and administrative perspective, giving me a deeper insight into how they function and are managed.
