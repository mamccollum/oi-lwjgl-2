DATE WRITTEN: 2024-11-24

This should build with OpenJDK 8 on OpenIndiana.
Note that setting your $JAVA_HOME may not be enough,
you might need to prefix your $PATH with the JDK8 bin directory.

Do note that if you are to use this with Minecraft,
only versions up to (and including) 1.12.2 are supported.
This is because 1.13+ relies on LWJGL3. I am unsure if it even supports Solaris at all.

You'll likely want OLauncher, as Prism is too buggy on Solaris:
 - https://github.com/olauncher/olauncher/releases

OpenIndiana.org IPS FMRIs used to build this:
 - pkg:/metapackages/build-essential
 - pkg:/developer/java/openjdk8
 - pkg:/runtime/java/openjdk8
 - pkg:/developer/build/ant
 - (... And probably a few I forgot ...)

Build process (replace the JDK path with your own):
 - export JAVA_HOME="/usr/jdk/openjdk1.8.0/bin"
 - export PATH="/usr/jdk/openjdk1.8.0/bin:$PATH"
 - ant generate-all
 - ant compile
 - ant compile_native

Note that I modified the build scripts to only build 64-bit binaries; this is because
OI does not ship 32-bit JDKs AFAIK anymore.

Install process:
 - sudo mkdir -p /opt/lwjgl-2.9.3/lib/amd64
 - sudo cp ./libs/solaris/lib*.so /opt/lwjgl-2.9.3/lib/amd64/
 - sudo chmod +x /opt/lwjgl-2.9.3/lib/amd64/lib*.so
 - sudo ln -s /opt/lwjgl-2.9.3/lib/amd64/liblwjgl64.so /opt/lwjgl-2.9.3/lib/amd64/liblwjgl.so
 
Running:
 - Prepend "-Dorg.lwjgl.librarypath=/opt/lwjgl-2.9.3/lib/amd64/" to your JVM arguments.
 - You can do this easily in the "Edit Profile" button in OLauncher for Minecraft
 --- Example as to what I did:
 --- EXECUTABLE    : /usr/jdk/instances/openjdk17.0.12/bin/java
 --- JVM ARGUMENTS : -Dorg.lwjgl.librarypath=/opt/lwjgl-2.9.3/lib/amd64/ -Xmx2G (...etc...)

Hope this comes in handy. :)

-- Molly
