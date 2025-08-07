# Java Installation (Linux)

- [Download \& Extract Java](#download--extract-java)
- [Setting up environment variables](#setting-up-environment-variables)
- [Verify installation](#verify-installation)
- [Appendix](#appendix)
  - [What is `/opt` folder?](#what-is-opt-folder)
  - [What is `/etc/profile.d` folder?](#what-is-etcprofiled-folder)
  - [What is `update-alternatives` command?](#what-is-update-alternatives-command)

### Download & Extract Java

- Download the desired JDK archive
  - [https://www.oracle.com/in/java/technologies/downloads/](https://www.oracle.com/in/java/technologies/downloads/)
- Run the following command to extract the zip file
  - `tar -xvzf jdk-17.0.15_linux-aarch64_bin.tar.gz`
    - `x` - Extract
    - `v` - Verbose
    - `z` - Compression type is Gzip
    - `f` - Specifies the next argument as filename
  - output folder: `jdk-17.0.15`
- Move the extracted JDK to `/opt`
  - `sudo mv jdk-17.0.15 /opt/jdk-17.0.15`

### Setting up environment variables

- Creating script to set `JAVA_HOME` path for system-wide access
  - `sudo nano /etc/profile.d/jdk17.sh`
  
    ```bash
    export JAVA_HOME=/opt/jdk-17.0.15
    ```

- Setting default JDK

  ```bash
  sudo update-alternatives --install /usr/bin/java java /opt/jdk-17.0.15/bin/java 1
  sudo update-alternatives --install /usr/bin/javac javac /opt/jdk-17.0.15/bin/javac 1
  sudo update-alternatives --config java
  sudo update-alternatives --config javac
  ```

  - Make sure to update the `JAVA_HOME` path as well while switching between different versions.

- Apply the changes
  - `source /etc/profile.d/jdk17.sh`

### Verify installation
  
- `java -version`

## Appendix

### What is `/opt` folder?

- `/opt` folder is a common place to keep manually installed software

### What is `/etc/profile.d` folder?

- It is a system-wide configuration directory containing shell scrips.
- These scripts are sourced (executed) by the login shell when a user logs in.
- It allows modular configuration of environment variables and shell behaviour for all users.
  - When a user logs in to the terminal, the system runs `/etc/profile`.
  - Above script sources (includes) all `*.sh` scripts inside `/etc/profile.d`.

### What is `update-alternatives` command?

- `sudo update-alternatives --install <generic_path> <name> <actual_path> <priority>`
- `update-alternatives` command is used to manage multiple versions of the same tool or command (like Java, Python, gcc etc.).
  - `--install` flag registers a new alternative of the tool/command to `generic_path` folder.
  - `generic_path` maintains the symlink to pointed version executable
    - When a user enters a command, shell looks up the `$PATH` environment variable.
    - Then it searches each directory in the `$PATH`, in order, for matching executable.
    - So make sure that `generic_path` exists in the `$PATH` (by default `$PATH` has `/usr/bin` folder registered).
    - `update-alternatives` manages symlink to this `generic_path` to point desired version.
      - `/usr/bin/java -> /etc/alternatives/java -> /opt/jdk-17.0.15/bin/java`
  - `name` name of the group (used in --config)
  - `actual_path` The specific executable that is installing as an alternative.
  - `1` Priority of this version (higher number = higher priority)
- `sudo update-alternatives --config <name>`
  - This command is used to switch between the registered alternatives.
