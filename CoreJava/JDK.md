#**JDK->**

Java Development Kit (JDK) provides environment and tools for developing, compiling, debugging, and executing a Java program.

Core components of JDK include:
JRE
Development Tools

*Development Tools->*
1. Basic Tools->
These tools lay the foundation of the JDK and are used to create and build Java applications. Among these tools, we can find utilities for compiling, debugging, archiving, generating Javadocs, etc.

They include:
javac – reads class and interface definitions and compiles them into class files
java – launches the Java application
javadoc – generates HTML pages of API documentation from Java source files
apt – finds and executes annotation processors based on the annotations present in the set of specified source files
appletviewer – enables us to run Java applets without a web browser
jar – packages Java applets or applications into a single archive
jdb – a command-line debugging tool used to find and fix bugs in Java applications
javah – produces C header and source files from a Java class
javap – disassembles the class files and displays information about fields, constructors, and methods present in a class file
extcheck – detects version conflicts between target Java Archive (JAR) file and currently installed extension JAR files


2. Security Tools->
These include key and certificate management tools that are used to manipulate Java Keystores.

A Java Keystore is a container for authorization certificates or public key certificates. Consequently, it is often used by Java-based applications for encryption, authentication, and serving over HTTPS.
Also, they help to set the security policies on our system and create applications which can work within the scope of these policies in the production environment.
These include:
keytool – helps in managing keystore entries, namely, cryptographic keys and certificates
jarsigner – generates digitally signed JAR files by using keystore information
policytool –  enables us to manage the external policy configuration files that define installation’s security policy


##Many things to check and update