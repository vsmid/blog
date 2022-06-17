 # Maven + mvnd + NetBeans + VS Code - speeding up local development experiment

 I like Maven. I am kind of used to working with it.
Gradle is faster which is especially noticeable when triggering local builds over and over again.

### Wouldn't it be great if local Maven builds could execute faster?

Not so long ago I stumbled upon [mvnd](https://github.com/apache/maven-mvnd)...

In a few words, faster builds and better console output. I played with it a bit and so far it delivers.

So to make this tool really useful for me I decided to delegate all my Maven commands to mvnd(run mvn but execute mvnd) and see what happens.
As a bonus, I was hoping that IDEs integrating Maven will also pick this up. And the ones I tried really did.

Mvnd embeds Maven but I already have Maven installed on my machine and a lot of tools are pointed to it so I wanted to leave it like that. Basically, I didn't want to mess with my current setup too much.
Since I use Mac, I will show only what I did on my macOS. It is fairly simple so it should not be too hard to reproduce it on some other OS.

* Install mvnd, I used brew 
    ```bash
    brew install mvndaemon/homebrew-mvnd/mvnd
    ```
* Edit active Maven mvn file(create backup of original file first :-)) which in my case was found at /usr/local/Cellar/maven/3.6.3_1/libexec/bin/mvn so that it looks like this: 
    ```bash
    #!/bin/bash
    JAVA_HOME="${JAVA_HOME:-/usr/local/opt/openjdk}" exec "/usr/local/Cellar/mvnd/0.0.12/bin/mvnd" "$@"
    ```
* on Mac, IDEs recognize the above libexec Maven directory as a valid Maven directory when installed with brew hence the change was made there

* 3.6.3_1 and 0.0.12 version values used in above code are my current versions of Maven and mvnd respectively. You can replace those with what you have installed on your machine


Now, with the above setup every time I run mvn from e.g. terminal, mvnd will be used instead. Also, any IDE which integrates Maven and is configured to use /usr/local/Cellar/maven/3.6.3_1/libexec/bin/mvn should also use mvnd instead. I tested this with NetBeans and it worked great. A project consisting of 6 Maven modules and 100+ tests went down from roughly 8 to 5 secs when using mvn clean install subsequently.
I also tried VS Code's Maven extension and it also worked as expected.

## Conclusion

As I didn't have much time to further experiment with this setup I guess there are better ways to do this but for me, the primary goal was to see whether I could speed up local builds using NetBeans.