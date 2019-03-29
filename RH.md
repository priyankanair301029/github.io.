

Questions:

As an administrator, how do I install and configure an Owncloud server?

As an administrator, how do I enable users to connect to the Owncloud server using the server's IP address and port 8080?

**Assumption: Installations are on a Windows system.**

Answer:

### Prerequisite:

- Check System Requirements: https://doc.owncloud.org/server/10.1/admin_manual/installation/system_requirements.html

- Create a Project Directory 

- Download docker-compose.yml from[ the ownCloud Docker GitHub repository](https://github.com/owncloud-docker/server.git).


### Procedure:

To install on your local system:

1. Create a .env configuration file, with the following configuration settings:

   | **Setting Name** | **Description**           | **Example** |
   | ---------------- | ------------------------- | :---------: |
   | OWNCLOUD_VERSION | The ownCloud version      |   latest    |
   | OWNCLOUD_DOMAIN  | The ownCloud domain       |  localhost  |
   | ADMIN_USERNAME   | The admin username        |    admin    |
   | ADMIN_PASSWORD   | The admin user’s password |    admin    |
   | HTTP_PORT        | The HTTP port to bind to  |    8080     |

2. Start the container as shown below using [Docker Compose](https://docs.docker.com/compose/).

   **Note**: You can use any Docker Command-line Tool of your choice.

```console
''''# Create a new project directory
mkdir owncloud-docker-server

cd owncloud-docker-server

# Copy docker-compose.yml from the GitHub repository
wget https://raw.githubusercontent.com/owncloud-docker/server/master/docker-compose.yml

# Create the environment configuration file
cat << EOF > .env
OWNCLOUD_VERSION=10.0
OWNCLOUD_DOMAIN=localhost
ADMIN_USERNAME=admin
ADMIN_PASSWORD=admin
HTTP_PORT=8080
EOF

# Build and start the container
docker-compose up -d'''
```

3. Run ***docker-compose ps*** to ensure the process is complete and that all the containers have successfully started. If successful, the following output is displayed:

```console
Name                Command                       State             Ports
__________________________________________________________________________________________
server_db_1         /usr/bin/entrypoint/bin/s …   Up                3306/tcp
server_owncloud_1   /usr/local/bin/entrypoint …   Up                0.0.0.0:8080->8080/tcp
server_redis_1      /bin/s6-svscan /etc/s6        Up                6379/tcp
```

The database, ownCloud, and Redis containers are running, and ownCloud is accessible via port 8080 on the host machine.

|      | Note: It takes a few minutes for ownCloud to be fully functional. You can use the *docker-compose logs --follow owncloud* command and see a significant  amount of information logging to the console, wait until it  slows down to attempt to access the web UI. |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

Question:

As an administrator, how do I add a user account?

Answer:

### Procedure:

1. Log in to the ownCloud UI, open `http://localhost:8080` in your browser of choice, where you see the standard ownCloud login screen.
2. The username and password are the admin username and password stored in the`.env file`.
3. To create a user account, enter a **Login Name** and a initial **Password** (cannot be "0").
4. The **Login Name** may contain letters (a-z, A-Z), numbers (0-9), dashes (-), underscores (_), periods (.) and at signs (@). After creating the user, you can either fill in their **Full Name** if it is different than the login name, or leave it for the user to complete.
5. (Optional) Assign **Groups** memberships.
6. Click the **Create** button.
7. If you have checked **Send email to new user** in the control panel, enter the new user’s email address, and ownCloud will automatically send them a notification with their new login information. You can also edit the email using the email template editor on your Admin page (see [*Email Configuration*](https://doc.owncloud.org/server/9.0/admin_manual/configuration_server/email_configuration.html)).

Question:

As a user, how do I connect to the Owncloud server using a desktop or mobile client?

Answer:

### Prerequisite:

- Check System Requirements: https://doc.owncloud.com/desktop/2.5/installing.html#system-requirements
- Download the  latest version of the ownCloud Desktop Synchronization Client from the [ownCloud Website](https://owncloud.org/install/#).

### Procedure:

1. In the Installation wizard, select Integration for Windows Explorer. 

   This allows users to share files directly from their local ownCloud folder in Windows Explorer, rather than having to open a Web browser and share from the ownCloud Web interface. 

2. In the next screen, select the installation folder for the client. 

   **Note:** The default location for the installation is fine; don’t change this without a good reason.

3. In the next screen, enter your ownCloud server URL.

4. Check *Trust this certificate*, if your ownCloud server has a self-signed SSL certificate.

5. In the next screen ,enter the ownCloud login and password.

   **Note:** The username and password are the admin username and password stored in the`.env file`.

6. Select folders and files to sync, and the location of your local ownCloud folder.

7. The  *Choose what to sync*  opens a file picker.

   **Note:** Any unchecked folders are removed from your local filesystem. On a new installation, when you have not yet synced with your ownCloud server, files are not deleted.

8. In the next screen, either click to open ownCloud in a Web browser, or open your local ownCloud folder.

   **Note:**  Open your local ownCloud folder to view file manager integration.  You can also right-click any file or folder, and then left-click “Share with ownCloud” to create a share link. The Share With option available in Windows, is not the ownCloud Share option. 





