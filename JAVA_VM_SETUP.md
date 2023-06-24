# Install JAVA Version Manager

```bash
$ sudo apt-get -y install zip unzip
$ sudo curl -s "https://get.sdkman.io" | bash
$ source "$HOME/.sdkman/bin/sdkman-init.sh"
```

You've now installed sdkman, you can check the sdk version with `sdk version`.

# Install JAVA version

```bash
$ sdk install java 17.0.7-zulu
$ java -v
```

# Change JAVA version

```bash
$ sdk install java x.y.z-zulu
$ sdk use java x.y.z-zulu
```

# Install Gradle

```bash
$ sudo wget https://services.gradle.org/distributions/gradle-8.1.1-bin.zip -P /tmp
$ sudo unzip -d /opt/gradle /tmp/gradle-*.zip
$ rm -rf /tmp/gradle-*.zip*
$ sudo nano /etc/profile.d/gradle.sh
```

Write the following into the file :
```md
export GRADLE_HOME=/opt/gradle/gradle-8.1.1
export PATH=${GRADLE_HOME}/bin:${PATH}
```

Add the permisions and activate gradle
```bash
$ sudo chmod +x /etc/profile.d/gradle.sh
$ source /etc/profile.d/gradle.sh
$ gradle -v
```

# Run a project with gradle

Go to the root of the project folder and run the following :

```bash
$ gradle build
$ gradle run
```

# Install PM2 (nodejs process manager) - Optional

You'll need to install nodejs.

```bash
$ sudo curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
$ source ~/.bashrc
$ nvm install v18.16.1
$ npm install -g pm2
```

Note: You can check the available node version with `nvm list-remote`.

# Run a Java project with PM2

Go to the root of your java project.

```bash
$ nano ecosystem.config.yml
```

Write the following to the file : 
```yml
apps:
  - name: my-java-app
    script: java
    args: ['-jar', '/path/to/your/app-1.0-SNAPSHOT-all.jar']
    interpreter: ''
    exec_mode: fork
    max_memory_restart: '1G'
    autorestart: true
    watch: false
    log_date_format: 'YYYY-MM-DD HH:mm:ss.SSS'
```

```bash
$ nano my-java-app.sh
```

Write the following to the file :
```bash
#!/bin/bash
java -jar path/to/your/app-1.0-SNAPSHOT-all.jar server config.yml
```

Then lastly, run the following :

```bash
$ chmod +x my-java-app.sh
$ pm2 start my-java-app.sh --name=my-java-app
```

Note: Necessary pm2 commands below
- `pm2 log`
- `pm2 log <pid>`
- `pm2 stop <name>`
- `pm2 list`