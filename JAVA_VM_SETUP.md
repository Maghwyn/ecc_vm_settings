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