This project aims to give you a very simple starting point for setting up a java project that you will build with ant. 
I will hopefully expand on it as I learn more about the magic of ant. :)

###Prequisites


	* You will need to have these packages installed to start properly rocking:
```
yum install java-1.8.0-openjdk java-1.8.0-openjdk-devel -y
```
###Setup the HelloWorld java application

We want to separate the source from the generated files, so our java source files will be in src folder. All generated files should be under build, and there splitted into several subdirectories for the individual steps: classes for our compiled files and jar for our own JAR-file.

Create the src directory for our project ( from what ever you java project root is ). 
```
mkdir -p src/test
```
The following simple Java class just prints a fixed message out to STDOUT, so just write this code into src/test/HelloWorld.java.
package test;

```
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```
###Compile and run using java commands

Compile and run the HelloWorld project:
```
mkdir -p build/classes
javac -sourcepath src -d build/classes src/test/HelloWorld.java
java -cp build/classes test.HelloWorld
```

The entire world should receive a greeting now:
```
Hello World
```


Creating a jar-file is not very difficult. But creating a startable jar-file needs more steps: create a manifest-file containing the start class, creating the target directory and archiving the files.

```
echo Main-Class: test.HelloWorld>myManifest
mkdir build/jar
jar cfm build/jar/HelloWorld.jar myManifest -C build/classes .
java -jar build/jar/HelloWorld.jar
```

###Anitfying the build process

After finishing the java-only step we have to think about our build process. We have to compile our code, otherwise we couldn't start the program. Oh - "start" - yes, we could provide a target for that. Weshould package our application. Now it's only one class - but if you want to provide a download, no one would download several hundreds files ... (think about a complex Swing GUI - so let us create a jar file. A startable jar file would be nice ... And it's a good practise to have a "clean" target, which deletes all the generated stuff. Many failures could be solved just by a "clean build".

By default Ant uses build.xml as the name for a buildfile, so our .\build.xml would be:
```
<project>

    <target name="clean">
        <delete dir="build"/>
    </target>

    <target name="compile">
        <mkdir dir="build/classes"/>
        <javac srcdir="src" destdir="build/classes"/>
    </target>

    <target name="jar">
        <mkdir dir="build/jar"/>
        <jar destfile="build/jar/HelloWorld.jar" basedir="build/classes">
            <manifest>
                <attribute name="Main-Class" value="oata.HelloWorld"/>
            </manifest>
        </jar>
    </target>

    <target name="run">
        <java jar="build/jar/HelloWorld.jar" fork="true"/>
    </target>

</project>
```
Now you can compile, package and run the application via ant:
```
ant compile
ant jar
ant run
```
or 
```
ant compile jar run
```

###External Documentation

There is SOOOO much more you can do with ant.  I've attached some docs to continue the learning process:


	* Most of the guts of this doc are stripped from this tutorial:
	* 
		* https://ant.apache.org/manual/tutorial-HelloWorldWithAnt.html



