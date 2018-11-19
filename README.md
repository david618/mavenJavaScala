# Overview
Create a Maven Project that includes both Java and Scala Code using (intellij)[https://www.jetbrains.com/idea/]


## Assumptions

You have installed Java and IntelliJ (Community Edition). During the install of IntelliJ add Scala support. 

I'm working in Linux (Mint 18); however, these instruction should work regardless of your operating system.

You have maven installed.  (e.g. apt-get install maven)

## Create New Project

From IntelliJ File -> New Project

Select Maven.  Do not select an archetype.  Just click Next.

Give your project a GroupID and ArtrifactID [Guide for Naming Conventions](https://maven.apache.org/guides/mini/guide-naming-conventions.html)

Give the Project and Name and Location. Click Finish.

You can import changes for the Maven projects or Enable Auto-Import.

## Create a Java Class

Create a package (e.g. org.jennings.java.learning)

In the package create a Java class.

```
package org.jennings.java.learning;

/**
 * Created by david on 6/1/17.
 */
public class Main {

    public static void main(String[] args) {
        System.out.println("Hello Java");
    }
}
```


## Create Scala Object

Right click on Project root folder and select "Add Framework Support"; select Scala

Under src/main create a new directory name scala; then right click scala directory select "Mark Directory As" Sources Root.

Right click on src/main/scala and create new Package (e.g. org.jennings.scala.learning)

In the package create new Scala Class (Change Kind to Object)

```
package org.jennings.scala.learning

/**
  * Created by david on 6/1/17.
  */
object Main extends App {
  println("Hello Scala")
}
```
## Configure for Scala and Java

Modified the pom.xml file.  Add the following xml.  

Some Tweaks 
- Replace mainClass with the name of your Main Class.
- Replace projectname with the name you want for your jar file


```
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <mainClass>org.jennings.java.learning.Main</mainClass>
        <projectname>mavenJavaScalalearning</projectname>
    </properties>


    <dependencies>
        <dependency>
            <groupId>org.scala-lang</groupId>
            <artifactId>scala-library</artifactId>
            <version>2.11.8</version>
        </dependency>

        <dependency>
            <groupId>org.scalatest</groupId>
            <artifactId>scalatest_2.11</artifactId>
            <version>2.2.1</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <finalName>${projectname}</finalName>

        <plugins>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>2.3.2</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>

            <plugin>
                <artifactId>maven-assembly-plugin</artifactId>
                <configuration>
                    <archive>
                        <manifest>
                            <mainClass>${mainClass}</mainClass>
                        </manifest>
                    </archive>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                </configuration>
                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>copy-dependencies</id>
                        <phase>prepare-package</phase>
                        <goals>
                            <goal>copy-dependencies</goal>
                        </goals>
                        <configuration>
                            <outputDirectory>target/lib</outputDirectory>
                            <overWriteReleases>false</overWriteReleases>
                            <overWriteSnapshots>false</overWriteSnapshots>
                            <overWriteIfNewer>true</overWriteIfNewer>
                        </configuration>
                    </execution>
                </executions>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <outputDirectory>target</outputDirectory>
                    <archive>
                        <manifest>
                            <addClasspath>true</addClasspath>
                            <classpathPrefix>lib/</classpathPrefix>
                            <mainClass>${mainClass}</mainClass>
                        </manifest>
                        <manifestEntries>
                            <Class-Path>lib/</Class-Path>
                        </manifestEntries>
                    </archive>
                </configuration>
            </plugin>

            <!--
            https://mvnrepository.com/artifact/net.alchim31.maven/scala-maven-plugin  << Last update Jun 2015

                                 Documentation: https://github.com/davidB/scala-maven-plugin
                                 David Bernard
                                 Previously Known as maven-scala-plugin
                                 Docs: http://davidb.github.io/scala-maven-plugin/

            -->
            <plugin>
                <groupId>net.alchim31.maven</groupId>
                <artifactId>scala-maven-plugin</artifactId>
                <version>3.2.2</version>
                <executions>
                    <execution>
                        <id>scala-compile-first</id>
                        <phase>process-resources</phase>
                        <goals>
                            <goal>add-source</goal>
                            <goal>compile</goal>
                        </goals>
                    </execution>
                    <execution>
                        <id>scala-test-compile</id>
                        <phase>process-test-resources</phase>
                        <goals>
                            <goal>testCompile</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <checkMultipleScalaVersions>false</checkMultipleScalaVersions>
                    <recompileMode>incremental</recompileMode>
                    <jvmArgs>
                        <jvmArg>-Xss1024k</jvmArg>
                        <jvmArg>-Xms64m</jvmArg>
                        <jvmArg>-Xmx2048m</jvmArg>
                    </jvmArgs>
                </configuration>
            </plugin>




        </plugins>
    </build>
    ```


Some Notes about pom.xml entries
- The properties section contains Maven variables that can be references in pom file; referenced with ${property-name}
- The org.scal-lang and or.scaltest are used to compile scala code in the project
- The maven-compiler-plugin, maven-assemply-plugin, maven-dependency-plugin, and maven-jar-plugin create jar files (one with dependencies and one without)
- The scala-maven-plugin compiles the scala code 


## Compile 
From command line enter the project folder.

<pre>
mvn install
</pre>

You should see output like this.

```
[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building mavenJavaScala-learning 1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ mavenJavaScala-learning ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 0 resource
[INFO] 
[INFO] --- scala-maven-plugin:3.2.2:add-source (scala-compile-first) @ mavenJavaScala-learning ---
[INFO] Add Source directory: /home/david/github/mavenJavaScala/mavenJavaScalalearning/src/main/scala
[INFO] Add Test Source directory: /home/david/github/mavenJavaScala/mavenJavaScalalearning/src/test/scala
[INFO] 
[INFO] --- scala-maven-plugin:3.2.2:compile (scala-compile-first) @ mavenJavaScala-learning ---
[INFO] Using incremental compilation
[INFO] 
[INFO] --- maven-compiler-plugin:2.3.2:compile (default-compile) @ mavenJavaScala-learning ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ mavenJavaScala-learning ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /home/david/github/mavenJavaScala/mavenJavaScalalearning/src/test/resources
[INFO] 
[INFO] --- scala-maven-plugin:3.2.2:testCompile (scala-test-compile) @ mavenJavaScala-learning ---
[INFO] No sources to compile
[INFO] 
[INFO] --- maven-compiler-plugin:2.3.2:testCompile (default-testCompile) @ mavenJavaScala-learning ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] --- maven-surefire-plugin:2.17:test (default-test) @ mavenJavaScala-learning ---
[INFO] 
[INFO] --- maven-dependency-plugin:2.8:copy-dependencies (copy-dependencies) @ mavenJavaScala-learning ---
[INFO] scala-xml_2.11-1.0.2.jar already exists in destination.
[INFO] scala-library-2.11.8.jar already exists in destination.
[INFO] scala-reflect-2.11.2.jar already exists in destination.
[INFO] scalatest_2.11-2.2.1.jar already exists in destination.
[INFO] 
[INFO] --- maven-jar-plugin:2.5:jar (default-jar) @ mavenJavaScala-learning ---
[INFO] Building jar: /home/david/github/mavenJavaScala/mavenJavaScalalearning/target/mavenJavaScalalearning.jar
[INFO] 
[INFO] --- maven-assembly-plugin:2.4.1:single (make-assembly) @ mavenJavaScala-learning ---
[INFO] Building jar: /home/david/github/mavenJavaScala/mavenJavaScalalearning/target/mavenJavaScalalearning-jar-with-dependencies.jar
[INFO] 
[INFO] --- maven-install-plugin:2.5.2:install (default-install) @ mavenJavaScala-learning ---
[INFO] Installing /home/david/github/mavenJavaScala/mavenJavaScalalearning/target/mavenJavaScalalearning.jar to /home/david/.m2/repository/org/jennings/learning/mavenJavaScala-learning/1.0-SNAPSHOT/mavenJavaScala-learning-1.0-SNAPSHOT.jar
[INFO] Installing /home/david/github/mavenJavaScala/mavenJavaScalalearning/pom.xml to /home/david/.m2/repository/org/jennings/learning/mavenJavaScala-learning/1.0-SNAPSHOT/mavenJavaScala-learning-1.0-SNAPSHOT.pom
[INFO] Installing /home/david/github/mavenJavaScala/mavenJavaScalalearning/target/mavenJavaScalalearning-jar-with-dependencies.jar to /home/david/.m2/repository/org/jennings/learning/mavenJavaScala-learning/1.0-SNAPSHOT/mavenJavaScala-learning-1.0-SNAPSHOT-jar-with-dependencies.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 3.328 s
[INFO] Finished at: 2017-06-01T13:45:57-05:00
[INFO] Final Memory: 24M/578M
[INFO] ------------------------------------------------------------------------

```

Notice the message that 1 Java and 1 Scala project were compiled.

## Run 

Run the Main Class specified in pom.xml
```
java -jar target/mavenJavaScalalearning.jar
Hello Java
```
Run a specific class.

```
java -cp target/mavenJavaScalalearning.jar org.jennings.scala.learning.Main
Hello Scala
```

