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