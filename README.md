# Welcome to the documentation page for SEPIA
Here you will (hopefully) find everything you need to know to get started with SEPIA.  

<p align="center">
  <img src="https://github.com/SEPIA-Framework/SEPIA-Framework.github.io/blob/master/img/SEPIA_ecosystem_w.png" alt="S.E.P.I.A. Logo"/>
</p>

Overview of SEPIA ecosystem (note: some parts are still in the dev branches).  
For image icon attributions please check the [homepage](https://sepia-framework.github.io/#attributions)

## Intro
**S.E.P.I.A. stands for server-based, extendable, personal, intelligent assistant** and is a modular framework for voice-assistants on the one hand and a ready-for-action digital assistant app that works cross-platform on browser, iOS and Android on the other hand. The server is based on Java and can be operated on Windows, Linux and Mac. Due to it's lightweight architecture it even works on a Raspberry Pi smooth and easy :relieved: :robot:  
SEPIA has smart-services for: **news, music (radio), timers, alarms, reminders, to-do and shopping lists, navigation, places, weather, Bundesliga soccer-results, Wikipedia, web-search and a bit of small-talk**. Some smart-home commands are prepared for integration as well and are waiting to get connected to other great open-source tools like Home Assistant (anybody wants to help? ^^).

### Architecture
The SEPIA Framework basically consists of 2 major parts: The [SEPIA Client](https://github.com/SEPIA-Framework/sepia-html-client-app) and the [Assist-Server](https://github.com/SEPIA-Framework/sepia-assist-server).  
  
**SEPIA Client:** The user interface that takes care of the speech-recognition to transform voice into text, sending that text to a SEPIA server for interpretation, receiving the result (in JSON format) and presenting it to the user via text, graphical elements and/or sound (text-to-speech). Typical clients can be iOS and Android apps, websites or even command-line tools combined with just a microphone.

**Assist-Server:** The "brain" of SEPIA that receives requests from the client via the HTTP REST API and takes care of the natural-language-understanding (intent and NER), conversation flow, smart-service integration (like a to-do list or news service), user-accounts and more. The Assist-Server can run on it's own Raspberry Pi or parallel to the client on more powerful systems (desktop PC ect.).  
  
Other notable components of the SEPIA Framework include a [WebSocket server](https://github.com/SEPIA-Framework/sepia-websocket-server-java) for chats and a [wake-word tool](https://github.com/SEPIA-Framework/sepia-wakeword-tools) for **"Hey SEPIA"** that even works on a Raspberry Pi Zero :relaxed: (thanks to Porcupine by Picovoice).  

## Languages
Currently SEPIA works in German and English with some services like news and soccer-results still optimized only for German meaning you will get an answer in English but see German news outlets for example or only Bundesliga results. The smart-services are constantly improving though.

## Wiki
Checkout the wiki for detailed descriptions:
[S.E.P.I.A. Framework Wiki](../../wiki)

## Quick-start (for users)
To use S.E.P.I.A. your personal, digital, open-source voice assistant you need 2 things:

1. One of S.E.P.I.A.'s client apps, e.g. the web version: https://sepia-framework.github.io/app/ (app store versions coming soon)
2. Access to a S.E.P.I.A. server. This can be [your own one](https://github.com/SEPIA-Framework/sepia-installation-and-setup), running e.g. on a Raspberry Pi or your Windows/Mac PC (see below) or you can find an open one hosted by a friend or a company (Note: the official SEPIA team is currently not hosting any public servers).

To connect to a custom server simply open the app, change the "hostname" in the log-in screen and restart the app. A typical hostname would be "my-server.example.org/sepia" or "[your-ngrok-url]/sepia". 

## Quick-start (for makers)
Release bundles and instructions for easy-installation of the S.E.P.I.A. server stack on Linux, Windows or Mac can be found here:
https://github.com/SEPIA-Framework/sepia-installation-and-setup  

Basically the steps are:
* Make sure you have Oracle Java 1.8 installed
* Download the latest SEPIA-custom-bundle from [here](https://github.com/SEPIA-Framework/sepia-installation-and-setup/releases/latest)
* Extract the zip and run "setup"
* Start the server (e.g. with the "run"-script) and continue with "quick-start (for users)" :-)

## Build-your-own (for experts)
Since everything in S.E.P.I.A. is open-source you can always build the whole framework from scratch using the Github repositories.
A first draft of the requirements to do so can be found [here](https://github.com/SEPIA-Framework/sepia-docs/wiki/Requirements).
This is (very) roughly what you need to do if you deploy all servers on one machine:

* Create a SEPIA folder and clone the core components of the framework:  
`git clone https://github.com/SEPIA-Framework/sepia-core-tools-java.git`  
`git clone https://github.com/SEPIA-Framework/sepia-websocket-server-java.git`  
`git clone https://github.com/SEPIA-Framework/sepia-assist-server.git`  
`git clone https://github.com/SEPIA-Framework/sepia-teach-server.git`

* Import each repository into your IDE (e.g. [Eclipse](https://de.wikipedia.org/wiki/Eclipse_(IDE)) as Maven-Project. Then export the servers as executable JARs manually (use .../server/Start.java as main method) or run `mvn install` for each repo in the same order as you cloned them (see above: core-tools, websocket, assist, teach).

* Copy your executable JARs, the 'libs' folder and the 'Xtensions' folder for each server to a new folder. Check out [these](https://github.com/SEPIA-Framework/sepia-installation-and-setup/tree/master/) examples for base folders. You will also find start, stop and other useful scripts to operate the server there.

* Get [Elasticsearch](https://www.elastic.co/products/elasticsearch) 5.3.3. ([official zip-file](https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.3.3.zip)) and run it on port 20724 (or change the default port in the config-files of the SEPIA servers, e.g.: [this one](https://github.com/SEPIA-Framework/sepia-assist-server/blob/master/Xtensions/assist.properties) and [this one](https://github.com/SEPIA-Framework/sepia-teach-server/blob/master/Xtensions/teach.properties))

* While Elasticsearch is running use the setup-function included in the Assist-Server to initialize the database, e.g.:  
`java -jar sepia-assist.jar setup --my`  
Note the flag **"--my"**! It can be used to switch server configurations between "--test", "--my" and "--live" (corresponding to the files in the 'Xtensions' folder) and has to be used when starting each server as well. The setup will also write some core accounts to your database and config files.

* If everything worked out you can boot up the whole server stack now. Start the database and servers in the following order:  
  * Elasticsearch (if not running already)
  * Assist-Server, e.g. `java -jar sepia-xyz.jar --my` (used by other servers get config data and authenticate users)
  * Chat-Server and Teach-Server (order does not matter here)
    
* Use one of the SEPIA clients to connect to your server stack. Inside the client you might want to set a proper host-name (see menu). If you are not running the client on the same machine you can replace 'localhost' by either an IP address or you can set-up your own proxy (e.g. [Nginx](https://de.wikipedia.org/wiki/Nginx)). Another great tool to use here is [ngrok](https://ngrok.com/docs) which works nicely together with the [SEPIA-reverse-proxy](https://github.com/SEPIA-Framework/sepia-reverse-proxy/releases/latest). More help about this part will be added soon!

## API keys for services
Some services integrated in SEPIA require an API key to run properly (e.g. Weather via forecast.io). Find out how to get them (for free) [here](../../wiki/API-keys).

## Final notes
If you run your own server and decide to open it to the public or to your friends please make sure to inform them about your data privacy policy since you are operating a database with user-accounts.
