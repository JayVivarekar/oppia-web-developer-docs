This guide provides step-by-step instructions for installing Oppia using Docker. Docker simplifies the installation process and ensures a consistent environment across different systems, making it easier to set up and run Oppia.

## Table of Contents

- [Docker - Brief Overview](#docker---brief-overview)
- [Installation Steps](#installation-steps)
  - [Clone Oppia](#clone-oppia)
  - [Install Docker Desktop](#install-docker-desktop)
  - [Start development server using `make` commands](#start-development-server-using-make-commands)
- [That's it! You have successfully installed Oppia using Docker, and ready-to-go for your contributions at Oppia.](#thats-it-you-have-successfully-installed-oppia-using-docker-and-ready-to-go-for-your-contributions-at-oppia)
- [Using Flags with Make Command](#using-flags-with-make-command)
- [Additional Make Commands](#additional-make-commands)
- [Contributing](#contributing)

## Docker - Brief Overview

Docker is an open-source platform that automates the deployment, scaling, and management of applications using containerization. Containers provide lightweight and isolated environments that encapsulate application dependencies and configurations, making installations more reliable and error-resistant.

Using Docker to install Oppia eliminates the need for extra configurations or setting up development environments for the local server. It provides a consistent and reproducible environment for running Oppia across different systems.

## Installation Steps

To install Oppia under Docker, follow these steps:

### Clone Oppia

1. Create a new, empty folder that will hold your Oppia work. Here, we call the folder `opensource`.

2. Navigate to the folder (`cd opensource/`). Next, we'll [fork and clone](https://help.github.com/articles/fork-a-repo/) the Oppia repository.

3. Navigate to https://github.com/oppia/oppia and click on the `fork` button. It is placed on the right corner opposite the repository name `oppia/oppia`.

   ![Screenshot with the fork button](images/install/fork.png)

   You should now see Oppia under your repositories. It will be marked as forked from `oppia/oppia`.

   ![Screenshot of repository list with Oppia](images/install/repositoryList.png)

4. Clone the repository to your local computer (replacing the values in `{{}}`):

   ```console
   $ git clone https://github.com/{{GITHUB USERNAME}}/oppia.git
   Cloning into 'oppia'...
   remote: Enumerating objects: 203313, done.
   remote: Total 203313 (delta 0), reused 0 (delta 0), pack-reused 203313
   Receiving objects: 100% (203313/203313), 179.26 MiB | 3.12 MiB/s, done.
   Resolving deltas: 100% (155851/155851), done.
   Updating files: 100% (4199/4199), done.
   ```

   > Note that you will see slightly different output because the numbers change as Oppia grows.

5. Now your `origin` remote is pointing to your fork (`{{GITHUB USERNAME}}/oppia`). To stay up to date with the main `oppia/oppia` repository, add it as a remote called `upstream`. You'll first need to move into the `oppia` directory that was created by the clone operation.

   ```console
   $ cd oppia
   $ git remote add upstream https://github.com/oppia/oppia.git
   $ git remote -v
   origin     https://github.com/{{GITHUB USERNAME}}/oppia.git (fetch)
   origin     https://github.com/{{GITHUB USERNAME}}/oppia.git (push)
   upstream   https://github.com/oppia/oppia.git (fetch)
   upstream   https://github.com/oppia/oppia.git (push)
   ```

   The `git remote -v` command at the end shows all your current remotes.

   Now you can pull in changes from `oppia/oppia` by running `git pull upstream {{branch}}` and push your changes to your fork by running `git push origin {{branch}}`.

   We have established a clean setup now. We can make any changes we like and push it to this forked repository, and then make a pull request for getting the changes merged into the original repository. Here's a nice picture explaining the process ([image source](https://github.com/Rafase282/My-FreeCodeCamp-Code/wiki/Lesson-Save-your-Code-Revisions-Forever-with-Git)).

   ![Diagram of the fork-and-clone workflow](images/install/forkCloneWorkflow.png)

   For making any changes to original repository, we first sync our cloned repository with original repository. We merge develop with `upstream/develop` to do this. Now we make a new branch, do the changes on the branch, push the branch to forked repository, and make a PR from Github interface. We use a different branch to make changes so that we can work on multiple issues while still having a clean version in develop branch.

### Install Docker Desktop
Download and install the latest version of Docker Desktop from the [official Docker website](https://www.docker.com/products/docker-desktop/). Docker Desktop provides a user-friendly interface and one must follow simple steps to download and install it from the given link.

> NOTE: The above step needs to be followed only once. The next time you want to run Oppia, you can directly start from the next section, i.e. by executing simple `make` commands from the root directory of the cloned Oppia repository.

### Start development server using `make` commands

1. **Navigate to Oppia Root Directory**: Open a terminal or command prompt and navigate to the root directory of the cloned Oppia repository.

2. **Build Oppia for the First Time**: Run the following command to build Oppia for the first time. This step downloads all the necessary third-party libraries, python dependencies, and the other services required for the Oppia development server, which may take approximately 15-20 minutes (varies according to the network connection speed).

   ```
   make build
   ```
> NOTE: This build is not related to production build by anyway. This command runs `docker compose build` under the hood, which is used to build the images for services defined in the `docker-compose.yml` file. For more information, refer to the [official documentation](https://docs.docker.com/compose/reference/build/).

3. **Start the local development server**: To start the local development server, execute the following command:

   ```
   make run-devserver
   ```

   This command launches the Oppia development server, and you can continue to perform your tasks as usual.

4. **Start the local development server in Offline mode**: To start the local development server in offline mode, execute the following command:

   ```
   make run-offline
   ```

   This command launches the Oppia development server in offline mode, and you can continue to perform your tasks as usual.
   > NOTE: Ensure that you have already built Oppia for the first time before running this command. If not, run `make build` first (with internet connection), which downloads all the necessary third-party libraries, python dependencies, and the other services required for the Oppia development server.

5. **Stop the Local Development Server**: To stop the local development server, execute the following command:

   ```
   make stop
   ```

   This command stops the Oppia development server and shuts down the Docker containers running the Oppia development server.

--------------------
That's it! You have successfully installed Oppia using Docker, and ready-to-go for your contributions at Oppia.
--------------------

## Using Flags with Make Command

You can use flags with the `make run-devserver` and `make run-offline` command to modify the behavior of Oppia development server as per the flag which is required to enhance your development workflow. The following table lists the available flags and their descriptions:
- `save_datastore`: This flag prevents clearing of the datastore upon shutting down the development server.
- `disable_host_checking`: Disables host checking so that the dev server can be accessed by any device on the same network using the host device's IP address. DO NOT use this flag if you're running on an untrusted network.
- `prod_env`: Runs the development server in production mode. This flag is useful for testing the production build of Oppia locally.
- `maintenance_mode`: This flag puts the Oppia development server into the maintenance mode.
- `source_maps`: Builds webpack with source maps.
- `no_auto_restart`: Disables the auto-restart feature of the development server when files are changed.


You can run the `make run-devserver` and `make run-offline` command with any of the above flags as follows:

```
make run-devserver save_datastore=true
```
or
```
make run-offline save_datastore=true
```

Similarly, you can use multiple flags as follows:

```
make run-devserver prod_env=true maintenance_mode=true
```
or
```
make run-offline prod_env=true maintenance_mode=true
```

## Additional Make Commands

The Oppia development environment provides additional `make` commands that you can use for various purposes. The following lists the available `make` commands and their descriptions:
- `make help`: This command shows the help menu for all the `make` commands for Oppia development environment under Docker.
- `make clean`: This command cleans the Oppia development environment (under Docker) by removing all the docker containers, images, and volumes. This command is useful when you want to start installation (under Docker) from scratch.
- `make logs.%`: This command shows the logs of the specified Docker container of the Oppia server. Replace `%` with the name of the service for which you want to see the logs. For example, `make logs.dev-server` shows the logs of the dev-server docker container of the Oppia server.
- `make shell.%`: Opens a terminal in the Docker container environment. Replace `%` with the name of the service for which you want to open the terminal. For example, `make shell.datastore` opens a terminal in the Datastore Docker container environment of the Oppia server.
- `make restart.%`: Restarts the specified Docker container of the Oppia server. Replace `%` with the name of the service for which you want to restart. For example, `make restart.dev-server` restarts the dev-server Docker container.
- `make update.requirements`: Updates all the Python dependencies of the Oppia server. Run this command when the local development server is running.
- `make update.package`: Updates all the npm packages of the Oppia server. Run this command when the local development server is running.
- `make build.%`: Builds the specified Docker container of the Oppia server. Replace `%` with the name of the service for which you want to build. For example, `make build.dev-server` builds the dev-server Docker container.
- `make stop.%`: Stops the specified Docker container of the Oppia server. Replace `%` with the name of the service for which you want to stop. For example, `make stop.dev-server` stops the dev-server Docker container.
- `make init`: Initializes the Oppia development environment by building the Docker Images and starting the dev-server.
- `make echo_flags`: This command shows the flags with thier values that are being used by the Oppia development server.

## Contributing

If you encounter any issues during the installation process or have suggestions for improvement, please feel free to open a discussion on [Oppia's GitHub Discussion](https://github.com/oppia/oppia/discussions)
