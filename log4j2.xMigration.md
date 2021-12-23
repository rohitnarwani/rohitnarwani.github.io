# Migrating from Log4j 1.x to Log4j2.x/Logback
## Approach - 1 
### Using the Log4j 1.x bridge
The simplest way to convert to using Log4j 2 is to replace the log4j 1.x jar file with Log4j 2's log4j-1.2-api.jar. You need to use below dependencies:
 
```xml <dependencies>
  <dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
    <version>2.17.0</version>
  </dependency>
  <dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.17.0</version>
  </dependency>
  <dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
    <version>2.17.0</version>
  </dependency>
</dependencies>
```
### To configure your log4j configuration via JVM argument you need to use below jvm arg:
```
-Dlog4j.configurationFile={path to file}
```

### In some cases if you are configuring by calling the classes DOMConfigurator or PropertyConfigurator. Then you need to remove that code.
You can use LoggerContext instead as shown below:
```
File file = new File("C:\\yourConfig.xml");
LoggerContext context = (LoggerContext) LogManager.getContext(false);
context.setConfigLocation(file.toURI());
```
### Then comes thhe main step in which you need to migrate from log4j to log4j2 - properties file configuration

* Convert from v1.2 properties to v1.2XML using this converter: https://log4j-props2xml.appspot.com/

* Convert from v1.2 XML to v2.0 XML (i.e. Log4j2.xml) using the procedure provided on this link: https://logging.apache.org/log4j/2.x/manual/migration.html

## Approach - 2
### Second approach would be without this bridge which would require changes in imports in all the classes. More details can be find here https://logging.apache.org/log4j/2.x/manual/migration.html. This would require lots of code changes.


## Approach -3
### Rather then upgrading or patching log4j2 to solve the security issue. Another approach is use it with an altrenative framework.One can use slf4j in conjuction with logback.

SLF4J ship with a module called <b>log4j-over-slf4j</b>. It allows log4j users to migrate existing applications to SLF4J without changing a single line of code but simply by replacing the log4j.jar file with log4j-over-slf4j.jar

The log4j-over-slf4j module contains replacements of most widely used log4j classes, namely org.apache.log4j.Category, org.apache.log4j.Logger, org.apache.log4j.Priority, org.apache.log4j.Level, org.apache.log4j.MDC, and org.apache.log4j.BasicConfigurator. These replacement classes redirect all work to their corresponding SLF4J classes.

To use log4j-over-slf4j in your own application, the first step is to locate and then to replace log4j.jar with log4j-over-slf4j.jar. Note that you still need an SLF4J binding and its dependencies for log4j-over-slf4j to work properly.

To use logback as the implementation use below dependencies: 


``` xml <dependencies>
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.9</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-core</artifactId>
    <version>1.2.9</version>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>log4j-over-slf4j</artifactId>
    <version>1.7.32</version>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.32</version>
</dependency>
</dependencies>
 ```
 
### To configure your logback configuration via JVM argument you need to use below jvm arg:
```
-Dlogback.configuration={path to file}
```

### For converting log4j properties to logback.xml use below utilities.
https://logback.qos.ch/translator/
