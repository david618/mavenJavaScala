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

<pre>
package org.jennings.java.learning;

/**
 * Created by david on 6/1/17.
 */
public class Main {

    public static void main(String[] args) {
        System.out.println("Hello Java");
    }
}
</pre>

## Create Scala Object

Under src/main create a new directory name scala; then right click scala directory and Sources Root.

Under src/test create a new directory name scala; then right click scala directory and Test Sources Root.

Right click on src/main/scala and create new Package (e.g. org.jennings.scala.learning)

In the package create new Scala Class (Change Kind to Object)

<pre>
package org.jennings.scala.learning

/**
  * Created by david on 6/1/17.
  */
object Main extends App {
  println("Hello Scala")
}
</pre>






## Configure for Scala and Java

Modified the pom.xml file.  Add the following xml.  

Some Tweaks 
- Replace mainClass with the name of your Main Class.
- Replace projectname with the name you want for your jar file


<pre>

    &lt;properties&gt;
        &lt;project.build.sourceEncoding&gt;UTF-8&lt;/project.build.sourceEncoding&gt;
        &lt;maven.compiler.source&gt;1.8&lt;/maven.compiler.source&gt;
        &lt;maven.compiler.target&gt;1.8&lt;/maven.compiler.target&gt;
        &lt;mainClass&gt;org.jennings.java.learning.Main&lt;/mainClass&gt;
        &lt;projectname&gt;mavenJavaScalalearning&lt;/projectname&gt;
    &lt;/properties&gt;


    &lt;dependencies&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;org.scala-lang&lt;/groupId&gt;
            &lt;artifactId&gt;scala-library&lt;/artifactId&gt;
            &lt;version&gt;2.11.8&lt;/version&gt;
        &lt;/dependency&gt;

        &lt;dependency&gt;
            &lt;groupId&gt;org.scalatest&lt;/groupId&gt;
            &lt;artifactId&gt;scalatest_2.11&lt;/artifactId&gt;
            &lt;version&gt;2.2.1&lt;/version&gt;
            &lt;scope&gt;test&lt;/scope&gt;
        &lt;/dependency&gt;
    &lt;/dependencies&gt;

    &lt;build&gt;
        &lt;finalName&gt;${projectname}&lt;/finalName&gt;

        &lt;plugins&gt;

            &lt;plugin&gt;
                &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
                &lt;artifactId&gt;maven-compiler-plugin&lt;/artifactId&gt;
                &lt;version&gt;2.3.2&lt;/version&gt;
                &lt;configuration&gt;
                    &lt;source&gt;1.8&lt;/source&gt;
                    &lt;target&gt;1.8&lt;/target&gt;
                &lt;/configuration&gt;
            &lt;/plugin&gt;

            &lt;plugin&gt;
                &lt;artifactId&gt;maven-assembly-plugin&lt;/artifactId&gt;
                &lt;configuration&gt;
                    &lt;archive&gt;
                        &lt;manifest&gt;
                            &lt;mainClass&gt;${mainClass}&lt;/mainClass&gt;
                        &lt;/manifest&gt;
                    &lt;/archive&gt;
                    &lt;descriptorRefs&gt;
                        &lt;descriptorRef&gt;jar-with-dependencies&lt;/descriptorRef&gt;
                    &lt;/descriptorRefs&gt;
                &lt;/configuration&gt;
                &lt;executions&gt;
                    &lt;execution&gt;
                        &lt;id&gt;make-assembly&lt;/id&gt;
                        &lt;phase&gt;package&lt;/phase&gt;
                        &lt;goals&gt;
                            &lt;goal&gt;single&lt;/goal&gt;
                        &lt;/goals&gt;
                    &lt;/execution&gt;
                &lt;/executions&gt;
            &lt;/plugin&gt;

            &lt;plugin&gt;
                &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
                &lt;artifactId&gt;maven-dependency-plugin&lt;/artifactId&gt;
                &lt;executions&gt;
                    &lt;execution&gt;
                        &lt;id&gt;copy-dependencies&lt;/id&gt;
                        &lt;phase&gt;prepare-package&lt;/phase&gt;
                        &lt;goals&gt;
                            &lt;goal&gt;copy-dependencies&lt;/goal&gt;
                        &lt;/goals&gt;
                        &lt;configuration&gt;
                            &lt;outputDirectory&gt;target/lib&lt;/outputDirectory&gt;
                            &lt;overWriteReleases&gt;false&lt;/overWriteReleases&gt;
                            &lt;overWriteSnapshots&gt;false&lt;/overWriteSnapshots&gt;
                            &lt;overWriteIfNewer&gt;true&lt;/overWriteIfNewer&gt;
                        &lt;/configuration&gt;
                    &lt;/execution&gt;
                &lt;/executions&gt;
            &lt;/plugin&gt;

            &lt;plugin&gt;
                &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
                &lt;artifactId&gt;maven-jar-plugin&lt;/artifactId&gt;
                &lt;version&gt;2.5&lt;/version&gt;
                &lt;configuration&gt;
                    &lt;outputDirectory&gt;target&lt;/outputDirectory&gt;
                    &lt;archive&gt;
                        &lt;manifest&gt;
                            &lt;addClasspath&gt;true&lt;/addClasspath&gt;
                            &lt;classpathPrefix&gt;lib/&lt;/classpathPrefix&gt;
                            &lt;mainClass&gt;${mainClass}&lt;/mainClass&gt;
                        &lt;/manifest&gt;
                        &lt;manifestEntries&gt;
                            &lt;Class-Path&gt;lib/&lt;/Class-Path&gt;
                        &lt;/manifestEntries&gt;
                    &lt;/archive&gt;
                &lt;/configuration&gt;
            &lt;/plugin&gt;

            &lt;!--
            https://mvnrepository.com/artifact/net.alchim31.maven/scala-maven-plugin  &lt;&lt; Last update Jun 2015

                                 Documentation: https://github.com/davidB/scala-maven-plugin
                                 David Bernard
                                 Previously Known as maven-scala-plugin
                                 Docs: http://davidb.github.io/scala-maven-plugin/

            --&gt;
            &lt;plugin&gt;
                &lt;groupId&gt;net.alchim31.maven&lt;/groupId&gt;
                &lt;artifactId&gt;scala-maven-plugin&lt;/artifactId&gt;
                &lt;version&gt;3.2.2&lt;/version&gt;
                &lt;executions&gt;
                    &lt;execution&gt;
                        &lt;id&gt;scala-compile-first&lt;/id&gt;
                        &lt;phase&gt;process-resources&lt;/phase&gt;
                        &lt;goals&gt;
                            &lt;goal&gt;add-source&lt;/goal&gt;
                            &lt;goal&gt;compile&lt;/goal&gt;
                        &lt;/goals&gt;
                    &lt;/execution&gt;
                    &lt;execution&gt;
                        &lt;id&gt;scala-test-compile&lt;/id&gt;
                        &lt;phase&gt;process-test-resources&lt;/phase&gt;
                        &lt;goals&gt;
                            &lt;goal&gt;testCompile&lt;/goal&gt;
                        &lt;/goals&gt;
                    &lt;/execution&gt;
                &lt;/executions&gt;
                &lt;configuration&gt;
                    &lt;checkMultipleScalaVersions&gt;false&lt;/checkMultipleScalaVersions&gt;
                    &lt;recompileMode&gt;incremental&lt;/recompileMode&gt;
                    &lt;jvmArgs&gt;
                        &lt;jvmArg&gt;-Xss1024k&lt;/jvmArg&gt;
                        &lt;jvmArg&gt;-Xms64m&lt;/jvmArg&gt;
                        &lt;jvmArg&gt;-Xmx2048m&lt;/jvmArg&gt;
                    &lt;/jvmArgs&gt;
                &lt;/configuration&gt;
            &lt;/plugin&gt;




        &lt;/plugins&gt;
    &lt;/build&gt;
</pre>


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

<pre>
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

</pre>

Notice the message that 1 Java and 1 Scala project were compiled.

## Run 

Run the Main Class specified in pom.xml
<pre>
java -jar target/mavenJavaScalalearning.jar
Hello Java
</pre>

Run a specific class.

<pre>
java -cp target/mavenJavaScalalearning.jar org.jennings.scala.learning.Main
Hello Scala
</pre>


