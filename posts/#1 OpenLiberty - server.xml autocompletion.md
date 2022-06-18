# OpenLiberty - server.xml autocompletion

**_Tue, Aug 18, 2020_**

I no longer use Eclipse IDE which means I no longer get to use Websphere Liberty extension for editing OpenLiberty's server.xml file. Love, love, no love, love, no love, no love, no love is how I would describe my relationship with Eclipse IDE but this is maybe a theme for some future article.
In this article I will show you how to easily have autocompletion capability while editing OpenLiberty's server.xml file.

As a first step, I consulted my good friend DuckDuckGo(that's right, "no Google for you" as Seinfeld's Soup Nazi would say). In the end, I couldn't find any useful tool but I was still very reluctant to use Eclipse IDE. So I started thinking about building my own tool. You could argue that this was a stupid idea and that I could have just stayed with Eclipse IDE for this particular case or just continued to read documentation and edit server.xml manually. And you would be right. There is really no need to be fancy about this, manually editing server.xml without autocompletion is not a big time consumer and is in the end perfectly fine but then again I am an engineer and a developer with a strong tendency for tool building(and also time wasting obviously).

I had a lot of ideas but the one worth mentioning(just for the story's sake, not because it was good) was revolving around scraping OpenLiberty's web page for documentation to feed my future autocompletion tool. You see how bad it can get? A scraping based tool for a problem of this magnitude is borderline insanity. The good thing though is I never really tried implementing any of my crazy ideas.

As the time passed by luckily I forgot about this whole episode until I again crossed paths with OpenLiberty. But this time, for reasons unknown to me a solution presented itself immediately without even thinking about it(Inception anyone?).

### If there is a XML could there be a XSD?


I couldn't find any references to XSD schema I could use but I did learn that OpenLiberty binary distribution comes with a tool which can generate one. The same is also true if you are using embedded OpenLiberty(via liberty-maven-plugin). After that, it was a one-way street for me.

To keep things short, if you want autocompletion while editing OpenLiberty's server.xml file and you are using editors like Apache NetBeans, Intellij IDEA or VS Code or any other tool capable of working with XSD schemas for that matter, you can simply include XSD schema in you server.xml file and the editor will take care of the rest.
 
### The Solution

* Download OpenLiberty binaries and extract the archive. Extracted archive should be found in the newly created wlp directory.

* Generate XSD schema by executing the following command(run the command in the same directory where wlp directory is created):

    ```bash
    java -jar ./wlp/bin/tools/ws-schemagen.jar ./wlp/usr/defaultServer/server.xsd
    ```
    Assuming you have Java installed on your machine, the above command will save generated XSD schema to /wlp/usr/defaultServer/server.xsd file. If defaultServer directory does not exist yet create it by starting OpenLiberty instance.

    For Maven based projects using embedded OpenLiberty ws-schemagen.jar can be found in ./target/liberty/wlp/bin/tools directory after Maven's package command is run. You should also set the location of generated XSD schema to a directory where you keep your server.xml(by default that is src/main/resources/liberty/config).

* Add schema location to <server> XML tag in server.xml file as shown in the snippet below:

    ```xml
    <server 
        description="my_server" 
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
        xsi:noNamespaceSchemaLocation="server.xsd">
        ...
    </server>
    ```

    Schema location is set to server.xsd because we saved it in the same directory where server.xml file resides. If you saved it somewhere else, make sure to adjust the noNamespaceSchemaLocation accordingly.

## Conclusion
Well, less is quite often more. Do not overthink and do not jump the wagon when you think you have a good idea. Let it brew for a while and then make a decision. 