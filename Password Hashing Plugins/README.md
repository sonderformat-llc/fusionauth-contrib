## Contributed Password Hashers

If you need to write a password hasher (also known as a password encryptor) to import users into FusionAuth without requiring them to change their password, you may use the contributed hashers in this project. This is intended to be used with the Writing a Plugin guide provided in the FusionAuth documentation.

The Writing a Plugin guide is found here:  https://fusionauth.io/docs/v1/tech/plugins/writing-a-plugin

## Example code

Any example password encryptors provided in this repository is provided to assist in your deployment and management of FusionAuth. Use of this software is not covered under the FusionAuth license agreement and is provided "as is" without warranty. https://fusionauth.io/license

This repository contains contributed and example hashers that should work with FusionAuth.

If you want a simple example to follow along in implementing your own hashing algorithm, please visit the [example plugin repository](https://github.com/FusionAuth/fusionauth-example-password-encryptor/).

## Build and test

- Clone this repository from GitHub.
- Then there are two ways to build this project, depending on if you have:
    - Java installed locally, or
    - Docker installed locally.
- Note at the time of writing that the `ExampleArgon2idPasswordEncryptorTest.java` tests fail in the VS Code dev container, and you need to comment them out to compile the project.

### Option 1: Build with local Java

Run the code in a terminal:

```bash
cd "Password Hashing Plugins"
mvn clean install
```

### Option 2: Build with Docker

If you build with Docker, the .jar file will be created in the container, but available on your host too, as the volume shares your local directory. There are two options: VS Code or manually using Docker:

#### If you are using VS Code, use the DevContainer

- Install the "Remote Containers" extension pack in VS Code
- After opening the repository in VS Code, at the bottom left, click the >< container icon and choose 'Reopen in container'.
- Your files should open in the sidebar. If they don't, open them manually from the file menu to `/workspaces`.
- Choose `Terminal` — `New Terminal` from the menu and enter:
    ```bash
    cd "Password Hashing Plugins"
    mvn clean install
    ```

#### If you are not using VS Code, use plain Docker

- Open a terminal in this repository directory and run the code below.

```bash
cd .devcontainer
docker build -t javaimage .
cd ..
docker run -it --name javabox -v .:/workspace javaimage

# inside the docker container:
cd "/workspace/Password Hashing Plugins"
mvn clean install

# /opt/savant-1.0.0/bin/sb build; # Savant fails to build, don't try this line.

exit
```

## Deploy the plugin

- Copy `fusionauth-example-password-encryptor-0.1.0.jar` to your FusionAuth `plugins` directory and restart FusionAuth.
    - If using FusionAuth in Docker, where the container is called `fa` this command will be:
    ```bash
    docker exec fa mkdir /usr/local/fusionauth/plugins &&
    docker cp "Password Hashing Plugins/target/fusionauth-example-password-encryptor-0.1.0.jar" fa:/usr/local/fusionauth/plugins/fusionauth-example-password-encryptor-0.1.0.jar
    ```

- Remember to start FusionAuth with Docker so the container exists for you to copy in the plugin, then stop and restart the container so that FusionAuth reboots and enables the plugin.

## OBSOLETE - Build with Savant

Savant fails with error `The license [PDDL-1.0] is not an allowed license type`.

**Note:** This project uses the Savant build tool. To compile using Savant, follow these instructions:

```bash
$ mkdir ~/savant
$ cd ~/savant
$ wget http://savant.inversoft.org/org/savantbuild/savant-core/1.0.0/savant-1.0.0.tar.gz
$ tar xvfz savant-1.0.0.tar.gz
$ ln -s ./savant-1.0.0 current
$ export PATH=$PATH:~/savant/current/bin/
```
