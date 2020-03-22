## Guide to Install GraalVM Community Edition on Ubuntu

1) Download the [latest release of GraalVM](https://github.com/oracle/graal/releases) and unpack it anywhere in your filesystem:


Note: GraalVM Community Edition 19.3.0 and later releases can be found [here](https://github.com/graalvm/graalvm-ce-builds/releases)


```
$ tar -xvzf graalvm-ce-java11-linux-amd64-20.0.0.tar.gz
```

2) Move the unpacked dir to `/usr/lib/jvm/` and create a symbolic link to make your life easier when updating the GraalVM version:

```
# mv graalvm-ce-java11-linux-amd64-20.0.0/ /usr/lib/jvm/
# cd /usr/lib/jvm
# ln -s graalvm-ce-java11-linux-amd64-20.0.0 graalvm
```

3) Add a new alternatives configuration. First grab the priorization number by listing the already installed JVMs and then use this number to configure the new one:

```
# update-alternatives --config java

There are 2 programs which provide 'java'.

  Selection    Path                                            Priority   Status
------------------------------------------------------------
  0            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      auto mode
  *1           /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      manual mode
```

In this case I have 2 java alternatives installed, so I'm going to install the third.

```
# sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/graalvm/bin/java 2
```

## Testing

To make sure everything is working fine, set the new JVM on your environment:

```
$ sudo update-alternatives --config java

There are 3 programs which provide 'java'.

  Selection    Path                                            Priority   Status
------------------------------------------------------------
  0            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      auto mode
 *1            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      manual mode
  2            /usr/lib/jvm/graalvm-ce-java11-20.0.0/bin/java   4         manual mode

Enter to keep the current selection[+], or type selection number: 2
```

To verify, just check the version number:

```
$ java -version
openjdk version "11.0.6" 2020-01-14
OpenJDK Runtime Environment GraalVM CE 20.0.0 (build 11.0.6+9-jvmci-20.0-b02)
OpenJDK 64-Bit Server VM GraalVM CE 20.0.0 (build 11.0.6+9-jvmci-20.0-b02, mixed mode, sharing)

```

And you're set.

## Note

The native-image executable is not bundled in the GraalVM distribution anymore. Install it manually using `$GRAALVM_HOME/bin/gu install native-image`.


## Unable to use `gu` command??

Solution: Export path in `bashrc` file 

Steps:

- Open bashrc file: `sudo nano ~/.bashrc`

- At the end add this: `export PATH=$PATH:/usr/lib/jvm/graalvm-ce-java11-20.0.0/lib/installer/bin`
  Note: You may use the suitable path as per your installation
  
  ### After that you may try running `gu` again: `gu install native-image` 
  
