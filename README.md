# Maven
programmer daily:
* writing code
* compiling code
* testing code
* packageing code
* deploying code

**Automate the task using maven**
 ## What is maven ?

* project management tool for JVM language
* major task
  * building source code
  * testing 
  * packageing into JAR WAR or EAR
  * generate Java Docs
  * manage dependencies
* also called **Build Tool** or **Dependency Management Tool**

## Install maven
1. go to website https://maven.apache.org/download.cgi
2. download maven
3. set envirnment variable as `M2_HOME`
   * create a new path as `M2_HOME`
   * then, add `%M2_HOME%` to path
4. check if `maven` is currently installed
5. open your terminal and type `mvn --version`, you should see the version information

**NOTE:**
when you use IntelliJ 2022.1 2021.3.2, you may see this error
https://youtrack.jetbrains.com/issue/IDEA-290419
you can try to install `apache-maven-3.6.3` instead.
you can find  `apache-maven-3.6.3`  following URL:
https://downloads.apache.org/maven/maven-3/3.6.3/binaries/
    

## Config maven in Intellij
1. go to Intellij `setting`
2. search for `maven`
3. go to the `Maven` option which under `Build Tools`   
4. set the `Maven home path ` to the directory where you download the zip file. 
5. and toggle the `show setting dialog for new Maven project` option
6. `apply` it then `OK`


## Create your first maven project (using IntelliJ)
1. create a new project
2. select `maven`
3. give it a name `maven-demo`
4. set the location 
5. and you can change artifact coordinates
   1. GroupID
   2. ArtifactID
   3. Versoion
6. then `finish`

## Maven folder structure
```txt
.
├── maven-demo.iml
├── pom.xml
└── src
    ├── main
    │   ├── java
    │   └── resources
    └── test
        └── java
```
`src`: the root directory of application and test unit
`main->java`: contain the source code of the application 
`main->resources`: contain the static files like xml file csv file, html  css file etc
`test`: unit test and integration tests application
`pom.xml` contains the metadata of the project dependencies
`target` not visble in current working directory, but it will contains all the compiled java class

## Maven core concepts
1. **pom.xml**
* `pom.xml` file stands for "Project Object Model"
   contains the metadata of the project and manage the dependencies
   Here is a `pom.xml` file look like
```xml
   <?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.clayliu</groupId>
    <artifactId>maven-demo</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
    </properties>

</project>
```

*different types of `pom` file*
* Simple POM
    the default pom file
* Super POM
    just a large pom file
    if you can unzip the file `maven-model-builder-3.8.5` first
    you can find the Super pom file in the `pom-install-directory/lib/maven-model-builder-3.8.5/org/apache/maven/model/pom-4.0.0`
* Effective POM
    Simple POM + Super POM file
    you can see it use `mvn help:effective-pom` in your project directory
    you can also go to the Intellij, right side click the maven icon,
    `right click` your project, and choose `Show Effective POM`

2. **Dependencies**
   **How install dependencies ?**
   step1: write a dependencies tag in your `pom.xml` file.
   ```xml
   <dependencies>
   
    </dependencies>
   ```
   step2:go to https://mvnrepository.com/ find the dependency you want to use
         click on the  spectifc version and copy it to the dependencies tag
         Here is use `junit ` as example
   ```xml
   <dependencies>
        <!-- https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter-engine -->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-engine</artifactId>
            <version>5.8.2</version>
            <scope>test</scope>
        </dependency>





    </dependencies>
   ```

   step3:click the maven icon or `Sync`, and then, the dependency will be automatically download.

   **And you can click the maven icon on the right side, there will be a dependencies, you can check it out to see if it already downloaded**


3. **Transitive Dependencies**
 - if you check the `Dependencies`
 - you can see the dependencies's depenceies
 - Over the time, the dependencies will be mess

**Eg** add another dependency
```xml
    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter-engine -->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-engine</artifactId>
            <version>5.8.2</version>
            <scope>test</scope>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <version>2.6.7</version>
            <scope>test</scope>
        </dependency>

    </dependencies>
```

- then take a look at the dependencies,
- you can see there is same dependencies  `junit`
[![OZib36.png](https://s1.ax1x.com/2022/05/04/OZib36.png)](https://imgtu.com/i/OZib36)
* we can set as following, to fix this problem
```xml

    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter-engine -->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-engine</artifactId>
            <version>5.8.2</version>
            <scope>test</scope>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <version>2.6.7</version>
            <scope>test</scope>
            
<!--            added 'exclusions' tag 'exclusion' tag
                and groupId
                to ensure which one we are using
                and make sure the following are in the  dependency you are using
-->
                <exclusions>
                    <exclusion>
                        <groupId>junit</groupId>
                        <artifactId>junit</artifactId>
                    </exclusion>
                </exclusions>

        </dependency>
    </dependencies>
```


4. **SnapShot and Release Dependences**

- SnapShot 
  - created when software is under development
  - unstable
- Release 
  - create after the software is developed and ready to be released 
  - Stable

4. **Dependency scope**
   - we can contorl the dependencies's visblity using dependencies
   - There 5 types of scope that maven have
        1. Complie
           - available only at compile time inside classpath
        2. Provide
           - provide by JDK or runtime, available at compile time but not runtime
        3. Runtime
           - available at run time but not compile time 
        4. Test
           - available at runnnig and writing tests.
        5. System
           - Path ot the JAR should be provided manually using `<systemPath>`

5. **Repositories**
- Local repositories
  - Folder inside the machine runnnig Maven
  - `<User-Home>/.m2`
- Remote repositories
  - remote website where can download dependencies
  - Eg. Maven Website Artifactory or nexus

```xml
    <repositories>
        <repository>
            <id>my-internal-site</id>
            <url>http://myserver/repo</url>
        </repository>
    </repositories>
```


6. **Build Lifecycle**
   [![OZV760.png](https://s1.ax1x.com/2022/05/04/OZV760.png)](https://imgtu.com/i/OZV760)
* Three steps:
  1. default
     1. validate:Verifies `pom.xml` is validate or not
     2. complie: Complie the source code
     3. Test: Runs the unit tests inside project  
     4. Package: packageing the source code into an Artifact
     5. Intergrtion-Test: execuse the Intergration Tests  
     6. Install install the created package into our local repository
     7. Deploy: deploys created package to the remote repository
  2. clean
     `maven clean install`
  3. site



7. **Plugins and Goals**
- Plugins enables us to return the lifecycle phases in our Maven Project
- Each Plugin is associated with a `GOAL`, which is linked to the lifecycle phase(ex:complie)
- Plugins can be defined inside `<plugin>` section, under `<bulid>` tag

8. **Maven Complie Plugin**
    * complie our java files, similar to running `javac <java-class-name>`
  * we can add plugins as following, we are using `maven-compiler-plugin` here
  ```xml
      <build>
        <plugins>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.10.1</version>
            </plugin>

        </plugins>

    </build>
  ```
  * we can added a class and `maven-compiler-plugin` will complie it and put it in`target` folder
  * command `mvn compiler:compile`
[![OZKUmt.png](https://s1.ax1x.com/2022/05/04/OZKUmt.png)](https://imgtu.com/i/OZKUmt)
* we can also test it using ` mvn compiler:testCompile`
[![OZMav9.png](https://s1.ax1x.com/2022/05/05/OZMav9.png)](https://imgtu.com/i/OZMav9)

