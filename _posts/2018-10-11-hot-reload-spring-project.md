---
layout: post
title: "Hot Reload Spring Project"
description: "Saving time using hot reload in Spring project"
category: 
tags: []
---
{% include JB/setup %}

Using [HotswapAgent](https://github.com/HotswapProjects/HotswapAgent)

### Specification
- Java 1.8.0_31 -> DCEVM-light-8u45-installer.jar (Try until column 'Installed altjvm' says YES)

![](https://raw.githubusercontent.com/hugo53/hugo53.github.io/master/images/dcevm.png)

### How to install

1. Download release of DCEVM Java patch based on Java version and launch the installer (e.g. sudo java -jar installer-light.jar). Currently you need to select correct installer for Java major version (7/8).
2. Select java installation directory on your disc and press "Install DCEVM as altjvm" button. Java 1.7+ versions are supported.
3. download latest release of Hotswap agent jar, unpack hotswap-agent.jar and put it anywhere on your disc. For example: C:\java\hotswap-agent.jar
4. Add ```-XXaltjvm=dcevm -javaagent:PATH_TO_AGENT\hotswap-agent.jar``` to VM options

##### Intellij IDEA

- Preferences -> Build, Excecution, Deployment -> Debugger -> Hot Swap:
	- Check "Build project before reloading classes'
	- Check "Reload classes in background"
	- Choose Always on Reload classes after compilation

![](https://raw.githubusercontent.com/hugo53/hugo53.github.io/master/images/idea-setup.png)
	
- Add ```-XXaltjvm=dcevm -javaagent:/sourcecode/hotswap-agent-1.3.0.jar``` to VM options in Configuration
- Press Command + Shift + F9 (Build -> Recompile ...) to reload only current file, or Command + F9 (Build -> Build Project) to reload whole project

![](https://raw.githubusercontent.com/hugo53/hugo53.github.io/master/images/idea.png)

- Change keymap:
	- Preferences -> Keymap -> Search for 'Compile', then change Command + Shift + F9 to Command + Shift + S to recompile easier 

### References
- [https://groups.google.com/forum/#!topic/hotswapagent/BxAK_Clniss](https://groups.google.com/forum/#!topic/hotswapagent/BxAK_Clniss)
- [https://www.javagists.com/spring-boot-devtools](https://www.javagists.com/spring-boot-devtools)
- [https://blog.philipphauer.de/increase-jvm-development-productivity/](https://blog.philipphauer.de/increase-jvm-development-productivity/)