Episode 6 - Maven with Brian Fox - https://manifest.fm/6
--------------------------------------------------------------

Andrew Nesbitt:     Welcome to The Manifest, a podcast all about package management. My name is Andrew Nesbitt.

Alex Pounds:        And I am Alex Pounds.

Andrew Nesbitt:     And together we're exploring the technical details of package management, the stories and the history of various projects and the communities around it. Before we get started, just a quick announcement. I'm helping to organize a package manager [inaudible 00:00:29] conference in Brussels next February. If you're interested in speaking, you've got until the 1st of December to submit to talk. You'll find a link to the [inaudible 00:00:37] proposal in the show notes. Today we're joined by Brian Fox, the co-founder and CTO at Sonatype, who maintain the Maven Central Repository. Brian, welcome to The Manifest.

Brian Fox:          Thanks for having me guys.

Andrew Nesbitt:     So Maven is in many ways a Java package manager, but it also encompasses some other languages too. Could you tell us some thing about that?

Brian Fox:          It's almost exclusively Java, but we do have people that have used it for various see libraries. I mean, a lot of the stuff also includes Java script. There's a web Java's project that puts Java script into Maven components and downloads them. But I mean, for the most part Maven and the Maven Central Repository is primarily Java. People on prem tend to use Maven for all kinds of crazy stuff. We've seen it used for movie files or firmware for fin set devices, some that aren't in business anymore, but we have seen it used in lots and lots of places.

Andrew Nesbitt:     So what about some of those more typical uses, how does Maven fit into the Java ecosystem?

Brian Fox:          Maven in many ways is almost synonymous with building modern Java applications. In the early days there was Ant, which is more of a procedural build system. Back then, I'm talking early 2000s, people would go, have to figure out the dependencies, they would download them, they'd throw him into a lib folder and tell Ant, just include star.JAR into your application. And everybody had multiple applications and they always included all these, of random JARs and nobody knew what they were, or why they were there and were afraid to pull him out because they might break at runtime.

Brian Fox:          So Maven came out of an Apache project called the Apache Turbine project, which actually doesn't exist anymore, but basically seeded many other Apache projects. And it was more of a declarative build system where you declared information about what you want using a convention over configuration approach and Maven has a life cycle. So if you say I'm building a JAR, it knows that you first need to pre-process resources, and then you compile them, and then you run the test steps, and those types of things, integration tests and ultimately then the packaging.

Brian Fox:          So Maven has a defined life cycle for all these things and plugins can attach themselves to various places in the life cycle. So it's sort of standardized. The build process made it easier to move from project to project. And also the thing that would probably be remembered forever, if you will, is it really brought the concept of being able to easily assemble applications from components because it brought the dependency management aspects.

Brian Fox:          So a particular library couldn't declare its own dependencies. And so then when you're assembling an application, those things are all pulled in transitively and Maven is able to resolve conflicts and all those kinds of things. So I think it was really Maven that pushed the envelope on that and you basically see those attributes and all of the modern package systems these days that people aren't downloading and throwing JARs into folders anymore, or JavaScript files into folders anymore. They're depending on the package manager to resolve it and all it's transitive dependencies. And I think that's really the thing that Maven changed about the Java ecosystem.

Andrew Nesbitt:     So Maven grew out of an Apache project, but now it's also kind of related to Sonatype the company you co-founded. Is it still an open source project? How do those two interrelate?

Brian Fox:          Great question. So Maven, the build system is still an Apache project and in the very early days, obviously no popular open source projects were, "Mavenized." And so we had to go through and write what's called the POM, which is really the build file that it stands for the project, object model. We had to figure out what the dependencies were and create those. And so it started out as people sharing the POMs for important things because that was what was required for Maven to process the transitive dependencies.

Brian Fox:          And in the super early days, that was in a giant subversion repository. But then people started including the JARs along with it. And so that, eventually was hosted at ibiblio, the university in North Carolina, then we pretty quickly, as it started to become popular somewhere in the 2004, 2005 timeframe, the bandwidth starting to exceed that and it was known to be unreliable. We became normal that downloads would fail just because we were overwhelming their connection.

Brian Fox:          And so one of the other Maven PMC members, one of the original guys, Jason van Zyl, he got a server on his own and basically started hosting that at different places at [inaudible 00:05:20] did a lot of donation for the bandwidth. And so the Central repository became this sort of separate thing. It was sort of tied to Maven because it was the primary place where you went to go get Mavenized objects, and it was also the default place where Maven projects would consume from and published too.

Brian Fox:          But it was always sort of separate from the Apache project, partially because in those early days when we were hunting around for a place to find it, it had things on it that had GPL license, which was antithetical to what Apache was all about. And so there was some back and forth over where that would be. And so then when we started Sonatype, we just continued maintaining that repository for the community, and it's grown tremendously since.

Brian Fox:          I mean, last year we did 52 billion downloads. I can remember the first time, I actually met Jason Face to face was in early 2007, I guess. And one of the things that I remember coming back with was a copy of Central on my laptop and it was maybe 20 gigs or something like that. It's quite a bit larger now. We're looking at 5.4 terabytes total space. A Maven coordinate consists of basically three parts, the group, the artifact and the version.

Brian Fox:          And so the group would be org Apache for example, and the artifact would be Struts or Maven or whatever. So the group and artifact uniquely identifies what you might consider to be a project. There's over 200,000 projects in Central. These days on a normal weekday, we're getting 3000 new releases pushed into the repository, which represent over 50,000 objects themselves each day growing at seven gigs, and we get about 30 new projects every single day onboarded. So for a repository now that's going on, what, 13, 14 years old, the fact that stuff is being added to it now more than it ever has in the past and that the consumption is still continues to go up, it's pretty awesome.

Andrew Nesbitt:     I guess you've obviously got the whole android ecosystem is pulling from the Maven repository with Gradle.

Brian Fox:          And it's interesting, I was looking at some of the statistics just earlier this week. Maven actually only represents 26% of the overall consumption from Central. And so that's why a lot of people mix Maven and Maven Central together because it kind of grew up and they're siblings, if you will. But Maven Central or what we call now just the Central Repository contains all of these things and it is produced and consumed by tools, not Maven.

Brian Fox:          There's a repository managers, there's Gradle, there's a Eclipse stuff and a full 50% is just rounded up for other. So you've got other builds systems Scala and Leiningen and Buildout and take your pick. But it's basically the place where people go to get Java components these days. It happens to be true that the metadata in there is a Maven format, but any modern tool understands that format. So really the Maven format is more popular than the Maven build tool itself these days.

Andrew Nesbitt:     With so many tools pulling from that Central repository, does that cause any problems for Maven?

Brian Fox:          Not really from the Central repository. At the end of the day, the repository was designed to be able to be operated as a static website. And so the tools themselves are producing and managing all the metadata and pushing it out there. This is distinctly different from most of the systems that have come since where they depend upon a service or a search. Those things tend not to be as easily scalable.

Brian Fox:          So in the very early days, and for many, many years, the Central repository quite literally ran from a single machine racked up in Central. We now have that machine at Sonatype headquarters in our little sort of museum. I wrote a blog post years ago when we de-racked it and they sent it to us. It's like, this is the first time I've seen this machine. I logged into this machine for years. It was just a blinking cursor to me. But I think it served Central for something like six years, without ever going down, which was somewhat amazing.

Brian Fox:          But now, we serve it through a CDN because we're doing such an enormous amount of bandwidth. It's 600 terabytes of bandwidth on a monthly basis that we're pushing now. And so clearly if we were trying to do every look up through a search, that would be ridiculous. But because the metadata is basically persisted in flat, it can pretty easily be distributed like a static website would behind the CDN.

Andrew Nesbitt:     I guess maybe a better question would be, does having so many tools pulling from that repository constrain you in some ways, particularly without Maven format, with so many tools, they've all got to be compatible and you might not be able to make the changes or update the format as you would be able to if it was just Maven?

Brian Fox:          So one of the annoying things I guess over the years, when Maven two came out, which was 2004, 2005 something like that, we versioned the XML structure as POM object 4.0.0, and we are still all these years later on 4.0.0 for basically that reason that if we were to change it, it would break compatibility with so many tools. And so in some ways consistency there has been great for growing the repo ecosystem, but has been a little bit of a ball and chain for the Maven project itself to be able to add new features.

Brian Fox:          If we want to change the way dependency resolution works and things like that, it can break the tools that don't understand it. And so we're working through different ways to be able to sort of decouple that type of information. But it will represent a bit of a shift in the repository and we're very mindful for tools like Python, had that kind of problem where it kind of split the community.

Brian Fox:          Some of us, we're working with the Jigsaw guys to try and help, as they're going through the Java nine modularization to sort of learn from what we've observed over the years of growing this repository and watched as other repo systems have had massive problems. NPM comes to mind where in the beginning it didn't have the scope and it didn't have, what Maven has is a group ID.

Brian Fox:          There was effectively a single global namespace. Pretty quickly they ran out of names, but also it creates other problems for organizations where somebody can pretty easily typosquat on a name, or if they know that a company might be using a name internally for example, they could go and try to register that on a public repo and hope that they accidentally download.

Brian Fox:          The lack of name spacing in and of itself can create huge potential risks in the way people are using these components and we wanted to make sure that Java and the Jigsaw stuff didn't create similar types of problems. So in some ways having all these different tools is awesome because it drives the stuff up.

Brian Fox:          The fact that we've been so stable in the metadata is a huge part of that. It is why 75% of the usage is tools, not Maven. The metadata for Maven is sort of like that universal barcode, the UPC code. Everything can read it, but if you were to invent a new one, forget about it. You wouldn't be able to sell your products. So it cuts both ways I guess.

Andrew Nesbitt:     Does the POM maximo format allow other tools to add in their own extra metadata or is it validated to ensure that it's only a certain set of allowed fields?

Brian Fox:          No, it's fairly strict in there. You can add properties in, but a lot of the elements in the POM are for Maven itself in terms of how to actually do the build. There is a section that lists out the dependencies, and some of the other nuances like if you're using properties and the dependency version for example, another tool has to understand how to do that. And we've created libraries and that's what the [inaudible 00:13:09] project is, a standard library. So tools don't have to implement the code over and over. They can just use that to get the right resolution.

Andrew Nesbitt:     The Maven project it's been around for a long time. How have you seen the development and the usage of it change over that period?

Brian Fox:          That's an interesting question. So we're on the third major revision of Maven, 3X. As you might expect for a project that's so old, the pace of innovation has slowed a little bit, but that's not always a bad thing because in the behavior stabilizes and people understand it, that was always one of the goals of Maven to make it easy. If you have never worked on a project before and you need to build it, and it's got all kinds of esoteric build scripts and weird system dependencies, that can be a nightmare.

Brian Fox:          But Maven basically strong armed people into not doing that so you can come along and build anything, if you know how to use Maven. And so the stabilization of that is a good thing, where people continue to innovate [inaudible 00:14:15] the plugins. It has thousands and thousands of plugins that do everything you can think of. I mean, those continue to be evolved quite a bit.

Brian Fox:          There is still a lot of core development going on within Maven performance improvements, trying to keep up with more modern CD pipelines, being able to do parallel bills and those types of things have been the stuff that's newish in the last say five years. But fundamentally, if you woke up from a coma since 2005 say and you know how to use Maven back then, you could use it today just the same way which I think is a good thing.

Brian Fox:          We've seen a lot of people move towards other build systems in Java. A lot of people tend to like Gradle these days. Gradle gives you a lot more flexibility. It depends on your perspective. I'm clearly biased having worked on the Maven project for so many years, I think that there is something to be said for that forced consistency, that too much flexibility is in fact problematic when people take it to the extreme. When they want to do something new and fancy, a build system is not the right place to do it, because of the next guy that comes along is going to go, what the heck is going on? How am I supposed to figure this out? How do I build this thing?

Andrew Nesbitt:     The people who have been listening to previous episodes, Gradle is one of those package managers that is during complete and allows you to execute arbitrary. I think its groovy code that the [inaudible 00:15:39] Gradle file is written in. So obviously I'm not a big fan of that because you can do some crazy things and it can be really hard to introspect and kind of trust the provenance of what actually came out the other side of it. Well, I'm going to ask you to play Gradle's advocate here. What kind of flexibility are people looking for that they might find in Gradle that they don't find in Maven?

Brian Fox:          That's a tough question. I think what tends to happen is that people that come at this, rather than stepping back and understanding, can I fit my build system into something that is recognized as the standard, they come at it more, how do I use this new tool to do the thing I'm already doing? And if you approach Maven with that mindset, you're going to lose. Maven is going to break your will or you're going to go find another tool.

Brian Fox:          And that's a little bit by design. We've always said Maven is very opinionated. I think that's the fundamental thing. If you were sitting down from the beginning figuring out, how do I just start building something from scratch? I think it's pretty easy to do that from Maven. But if you're coming at it with a project that's got 10 years of crazy Ant builds and other stuff like that, and you want to do all of those things with Maven, that can be a challenge.

Brian Fox:          If you're willing to roll up your sleeves and write plugins and sort of work within the standard frameworks, totally achievable. But if you're coming at it from a scripting mindset or something like that, you're going to run into trouble. And so I think that's where people tend to look for other solutions. But again, that exact power can lead to on maintainability down the road. So you have to be careful.

Andrew Nesbitt:     Especially when you're looking at, well, we want to run this Gradle file with the next version of Gradle, or a different version of groovy that turns out you can't actually work out what the dependencies are any more programmatically, and to boot strap your package manager, you actually need to run the package manager potentially.

Brian Fox:          Yes, that's right.

Andrew Nesbitt:     Talking of plugins, does the Maven project ever bring any of the really popular plugins in line and make them a piece of the main project, or is it a case where they always left kind of like as plugins?

Brian Fox:          Well, actually most of Maven itself is composed of plugins. There's just a couple of core things and all of the key things like compilation, packaging, resource processing, all the things that bind into that lifecycle that I described before, there are a set of default plugins that match to that, but as a user it's as easy to pull in any other plugin as it is to use the core plugins. So there're pretty much is no in or out group in terms of plugins in that ecosystem.

Andrew Nesbitt:     And that default set is chosen to be kind of a minimal feature set or is it the kind of best practice?

Brian Fox:          I would describe it more as the minimal set. So you need something that knows how to call the javac compiler for example. You need something that knows how to package into the JAR or the WAR or whatever it may be. Depending on what the component is that you're trying to build, the life cycle is slightly different and the default plugins that get mapped into that lifecycle will be different.

Brian Fox:          For example, if the POM declares a WAR, then it's obviously going to call the WAR plugin and not the JAR of plugin or the zip plug in to do it. But part of what you define in your own POM when you're building your own project are what other plugins need to be attached at different places in the life cycle. You can say, I want this plugin to run at the pre-compiled phase, or post compiled phase, or the test phase, or whatever lifecycle you want. And so, it's pretty straight forward.

Andrew Nesbitt:     For the non Java developers, could you just explain what a WAR is? Because you said if your file declares a WAR and it's not sparking international conflict.

Brian Fox:          No, not that. WAR stands for web archives. So, a WAR would be the file you deploy into a Java application container like Tom Cat. It has a bunch of JARs in it, but it also probably has HTML and maybe some JavaScript and CSS in a defined location. So that would get unpacked and the web application server knows exactly what to do with it. So you start at the lowest level you have JARs, which are literally just all of the bike code, organizing the class files. The next level up would be a WAR, and an EAR, an enterprise archive is comprised of multiple WARs. I don't think there's one that gets bigger than that.

Andrew Nesbitt:     And they're all really just fancy zip files, right?

Brian Fox:          Yes. They are all just zip files with a defined standard and metadata inside them. Yes.

Andrew Nesbitt:     I guess the next level up from that is then a docker container?

Brian Fox:          Good point. Yes. A docker container with the application server and maybe an EAR.

Andrew Nesbitt:     So do you have a particular favorite plugin?

Brian Fox:          I'm biased. I wrote two popular plugins, one that's called the dependency plugin and that gives you all kinds of ways to manipulate dependencies. But probably my favorite might be the enforcer plugin. Way back in the day I was having trouble with developers that I worked with following simple things like making sure they were running the right version of Maven or that they had the right version of Java or all kinds of things.

Brian Fox:          And so I created the enforcer plugin and we jokingly call it the loving iron fist of Maven. And it allows you to create your own kind of rules and plug them in. And so you can basically enforce pretty much anything you want and break a build. So those are my two favorite ones. They also tend to be really, really popular. And so even though I think it's been, oh geez, maybe 10 years since I contributed code to either of those plugins, I still, when I'm at customer sites, see them and use all the time. That's kind of cool.

Andrew Nesbitt:     That reminds me a lot of the episodes we recorded with [inaudible 00:21:32] and his credit danger, which is essentially the enforcer plugin, but run as part of your GitHub pull request process.

Brian Fox:          Right. Yes. Very similar.

Andrew Nesbitt:     So you've been involved in running Maven for a long time now. Do you have any interesting WAR stories or things that are kind of cause you to pull your hair out in the process of running it?

Brian Fox:          Absolutely. In the early days I kind of was spending a lot of time as the main maintainer of the repository. Certainly when it became the Sonatype years in 2007 and to maybe 2010 or so, I was like the guy running it. It was a daily battle preventing abusers. We always had people that would come along and just think like, I don't know, I'm just going to recursively [inaudible 00:22:19] get this repository.

Brian Fox:          And even back then it was quite large. And as a startup it was expensive to provide that even though [inaudible 00:22:27] was giving us a good deal on the bandwidth. There was one particular time where I think our bill jumped from, I don't know, a couple thousand dollars a month to $15,000 the next month. That was a big shock for us. We started looking into it and the traffic was spiking and a lot of interesting stuff, and we ended up having to move from Apache HTTP to Nginx because, the number of connections coming in at certain times were just simply overwhelming the system.

Brian Fox:          And so we sort of had this problem where it kind of built and it built and it built until Thanksgiving came along. And I really started looking into what was going on. I can remember sitting on the couch at my mother in law's house for an entire Thanksgiving, only stopping to go have dinner and then go sit back down at the computer and trying to figure out what was going on. And it was kind of an interesting thing because it was clearly time based that if you looked at the spikes, it was almost on the hour every hour, but there was a distribution to either side of the hour. So it looked a nice, sine wave basically.

Brian Fox:          And what we were seeing, and I'm trying to filter at all these different ways, trying to figure out what is it, or is it coming from these IP numbers, is it coming from certain tools? We were able to figure out after a while that the usage was coming from people downloading the index. So one of the artifacts in the repository, it's still there today, is actually a leucine index of everything in there. And a lot of tools make use of that for searching.

Brian Fox:          And so we could see that it was that, but we couldn't figure out why. It was from no Central place, no one single IP number or provider or anything like that. And worse, all of the Java agents just said Java. It didn't say like Nexus that we knew it wasn't Maven. We were trying to figure out what it was, and it turns out that it was a repo manager. There was a new version that had been released and over a couple of months it became more and more popular, which explained why this stuff was growing so sporadically.

Brian Fox:          And there was a bug that every time, every hour when it was supposed to check to see if there was an update to the index, it would just download it anyway. And so if you take that and you account for clock drift on people's computers, that explained the sine wave. So it took a long time because there wasn't any information in the request to figure it out. But we did eventually figure it out. But it quite literally took me an entire Thanksgiving to figure that out.

Andrew Nesbitt:     So what was the solution?

Brian Fox:          We figured it out. We blocked the nonspecific Java agent from hitting the index and then talked to those guys and got the bug fixed. And then we've kind of put out a message to tell all the users like, hey, can you change the setting and your config so you stop dossing Central for us. Thanks. Because it was costing us a lot of money in bandwidth. So that's one that I always remember.

Brian Fox:          There was another one, this was probably 2010, 11, maybe. We were getting a lot of spikes in the bandwidth and trying to figure it out and scratching our heads. And somewhat randomly, one of our developers found on a Minecraft forum that there was some modern kit that had changed their default config to point that Central for Scala. We had seen this Scala JAR being downloaded over and over again and I knew some of the guys over there.

Brian Fox:          So I asked them like, "Hey, can you think of a reason why anything would be downloading the Scala JAR like all the time?" And he's like, "No, I don't even know why anybody would be using that. That's like four or five versions old. There's no reason for that." And so after we dug through it, it turns out it was the modding framework for Minecraft had a bug in it, similar to the index where instead of just checking for an update, it was downloading it every single time.

Brian Fox:          So everybody who had a modded version of Minecraft anywhere in the world, when they started it up, it was downloading the Scala JAR from Central. And as we dug through it, it turns out, well they had [inaudible 00:26:17] their own servers accidentally and they were like, well this is costing us too much. Let's just change the URL to download from Central or something like that.

Brian Fox:          And so we eventually worked with them, we got that fixed and then it reared its head again probably like nine months, 12 months later as somebody had accidentally forked the old modding one and like re-released it. And so we had to chase that down. So that becomes daily life when you're a big giant public repository like that. You get all these weird things and you just, you can never figure out what people are intending to do, but yet you're on the receiving end of it.

Andrew Nesbitt:     [inaudible 00:26:48] that is unique to your project.

Brian Fox:          Yes. That certainly helps.

Andrew Nesbitt:     We had that similar thing with my [inaudible 00:26:58] became the search API for Bower, the package manager a little while back where they turned off their search once they realized that ours was considerably better and it wasn't costing them any money, which in itself was not too bad. The elastic search cluster was fine. The problem was when Microsoft implemented auto complete on package names inside of visual studio for Bower projects.

Andrew Nesbitt:     So anytime anyone typed inside of a bower.jason File, it hit the library search API, and as that a new version of visual studio rolled out, you could see on the metrics for hitting the search API that it got very aggressive very quickly and then just smashed our poor elastic search cluster into the floor.

Brian Fox:          Sounds like you're part of the club. That sounds exactly like things that have been happening to us for years.

Andrew Nesbitt:     We weren't even serving anything. That was purely just some Jason of the search response.

Brian Fox:          That's right. It's like when you're releasing software and it's talking to your server and all of a sudden something goes weird, you can kind of correlate it to, we released that version and this started happening, but when you're only seeing one side of the story, like Microsoft released their IDE, you didn't know that was coming, and it didn't happen all on day one. It's as people update. So it builds slowly.

Andrew Nesbitt:     And they hadn't really announced it as a feature. It just kind of popped up. It was an interesting one to debug. We actually managed to get better technical support via Twitter than via any kind of official means.

Brian Fox:          I mean, back in the day when we were dealing with constant crawling, I spent many, many nights and weekends, looking at scripts and trying to figure it out because I was trying to figure out how do I stop the crawlers from costing us money and slowing down the service without interfering with legit traffic. And that was a challenge. But I came up with some ways. I put some artifacts in the repository. They were basically just like Hello, World, but that were never advertised.

Brian Fox:          And so nobody in their right mind would ever download that. They were literally honeypots. In fact, they're still there, they're called nectar buckets. That was my joke. Hint, if anybody was actually paying attention, they'd probably figure out what it was. But the scripts when they would see somebody hit that, it would fire off an email to me with statistics about what was going on. So I could see like, this person was downloading things alphabetically, or you'd see him try to be devious and try to randomize it, but you could still tell.

Brian Fox:          And so after a while I got tired of that and basically made it a recursive black hole that once you went into one of those things, if you were trying to crawl it, you would crawl in infinite circles and it would just slow down and stop. So either I would get to it and shut them off or they would eventually figure it out and stop and then come email us and be like, hey, how do I do X, Y and Z? We don't have any of those in place anymore because of CDN, but sometimes it was fun and sometimes it was pull your hair out to try and save the bandwidth.

Andrew Nesbitt:     One common thread in all of these WAR stories is costs. How is Maven funded both in terms of funding development and also funding that ongoing hosting?

Brian Fox:          Well, Maven itself is an Apache software foundation project, and it's all open-source. It's all people that come and sign up and contribute, get karma. The projects themselves are run by what are called the project management committee, the PMCs, which are sort of like people had been doing it for a while. So that's how that happens. It's mostly just complete contributions.

Brian Fox:          Sonatype still funds the Central repository, we pay for it. It gets pretty expensive, but it's something that we do for the ecosystem and it's important for us to keep that going. So we spend a lot of time, we have a team that works on it. As you can imagine, the scale of these things get bigger, the abuse gets bigger, all those types of things.

Andrew Nesbitt:     I imagine you probably have the most people working on a package manager registry, perhaps maybe outside of the [inaudible 00:31:03] and system level package managers.

Brian Fox:          I mean, in terms of the total throughput besides maybe like you said, the Unix ones, I have to think it's the largest. The npm repository in terms of actual components downloaded is larger, but that's because they're like micro components or one JavaScript file. It would be like the equivalent of downloading a single class file from Maven. So I think last year npm did something like 50 billion components. We did 52 billion. But they're different sizes. The bandwidth is totally different.

Andrew Nesbitt:     So if somebody didn't trust in the longevity of Sonatype on, therefore the Central repository, does Maven have many options for pointing at their own local mirror? Can you do that?

Brian Fox:          Sure. That's one of the best practices actually. There's repository managers, it's one of the first products that Sonatype had, and besides the Maven training in the early days, and that's an on premise local cashing proxy basically. So it becomes your way of decoupling yourself from, internet outages and it just makes sense to not have all your thousand developers downloading the same thing over and over and over again. And so you would do that.

Brian Fox:          It also becomes a place where you host and share your internal artifacts. So in terms of like transient outages, internet, that kind of stuff with a CDN these days, Central doesn't really go down, certainly not it did in the early days. But we also maintain relationships with the Maven PMC for mirroring backups and things like that. And there's a mirror at Google that Google provides as well. It's not used a lot to my knowledge. It's more just there as a backup just in case kind of thing.

Brian Fox:          But it is very easy to point your Maven build at a different repository. It just tends to be that most people use Central because that's where everybody is publishing to. All of the different open-source projects are basically funneling through either one of the forges or they come to a Nexus repository that we maintain. That's how they put stuff into Central. So everybody goes there because it's sort of the canonical source, if you will.

Andrew Nesbitt:     So talking of publishing to a Central repository, I found it interesting that Maven has quite a lot of requirements compared to a number of other application [inaudible 00:33:17] package managers that basically allow you to publish anything with very little actual human interaction. Can you talk a little bit about the decisions behind that?

Brian Fox:          You're begging me to get on a soapbox here. So in the early days of Maven we maybe accidentally, maybe with a little bit of foresight, we kind of set down some expectations of how the repository would work. In fact, it's baked into Maven itself. That releases are immutable. Once you put a version number out there, it can't be reused. Even if you were to change it on the repository, all of the assumptions within Maven and the way it [inaudible 00:33:55] things locally, it's not going to work right. It doesn't expect it.

Brian Fox:          And so, we set out from the onset that the Central repository was an immutable store. I think it has to be that way, that's why it's so popular because people know it's effectively like the Library of Congress and web archive all in one for Java stuff. You know what's going to be there. So that was one of the initial standards. The other thing was that we required the group ID, which is the namespace to be something that you controlled so that it wasn't just random that somebody couldn't show up and pretend to be Eclipse.

Brian Fox:          And so those were standards that were put in place in the early days as was making sure that the artifacts themselves were signed with PGP signatures. Now it's true that not everybody goes and actually validates the stuff there, but we at least require that you put it in there. We figure if you're going to share your stuff for people to execute on their machines and in their builds and in their spaceships and all the stuff that literally this stuff ends up in, you should care enough to at least put a signature on it so that we can validate it hasn't been mutated.

Brian Fox:          And so we got those things right years and years ago, and I think when you start to look at what's going on with application security these days and how people are attacking the repositories with like the typosquatting and the non immutability that we saw with left pad and npm, all of those things sort of, I look at those and get really frustrated because it's like, man, if you just followed what was there before, and I feel in sometimes these new package managers come along and repeat mistakes of the past that were completely avoidable. And so there's a lot of requirements in to get things into Maven Central and I don't apologize for it, and I think the community has come to expect it.

Andrew Nesbitt:     Maven is used in spaceships?

Brian Fox:          Not necessarily Maven, but applications built by Maven certainly. I mean, we know that Eclipse stuff, was sent on the curiosity rover, and so I'm sure a lot of those dependencies that were shipped in there came from things built by Maven. No question about that.

Andrew Nesbitt:     This is actually a nice segue into a question I had about the long running nature of the Maven projects and all of that history, because as you said before, it is used in all of these different places and often a lot of different platforms. There's a lot more web apps out there now. Android didn't exist when Maven was created, and in some senses the package manager doesn't care. The package manager is an infrastructure tool and the same with an IDE doesn't care. Vim doesn't care that you're writing a mobile app or a web app or a desktop app. But do those different platforms have any impact on either Maven itself or the decisions you make around the project?

Brian Fox:          I don't think they have an impact on Maven itself other than, understanding how people want to develop modern applications and the web JARs, plugins and things like that was a response to trying to bring some of the Maven stuff to npm or JavaScript dependencies. I don't know exactly the history which came first, but you see a lot of that. It certainly affects how we think about the things that go into Central of the stability of Central, knowing that it is literally a worldwide resource for Java development.

Brian Fox:          We take that very, very seriously in terms of what we let go in, how we pay attention to it and all those kinds of things. And over the past few years, more and more focus has been placed on application security and the security of the components and those kinds of things and it's a big deal. Earlier this summer, there was a report that a researcher had done. It was for npm, but I think it could apply to anything where they were looking at leaked credentials.

Brian Fox:          People had checked in their npm credentials into GitHub and or we're just using a super simple brute forcible attacked passwords. And so that's sort of evidence that, when you're a component developer and a publisher, you're not really thinking down the road of what tremendous responsibility you have or you should take for those things because, the left pad is the perfect example. It's a super simple thing, but yet the disappearance of it from the repository broke, builds all around the world.

Brian Fox:          And so imagine what happens if somebody steals your credentials and publishes malicious versions of legitimate components into the repository, that could easily find its way into millions of applications nearly overnight. And so there is a sense of responsibility that clearly we have as we maintain that repository.

Brian Fox:          But I think it's important for people that are publishing into it to really, really think, how do they take their security seriously that their laptop at home is creating components that could be run on airplanes, on the financial system. I mean, it happens every single day and I don't think people really think about that. Because if they did, if they realize the responsibility that they had, they might act differently.

Andrew Nesbitt:     Talking about security. When a vulnerable version is found on the Maven repository, [inaudible 00:38:55] it's an immutable system, how would you go about informing users or trying to help them avoid those insecure versions?

Brian Fox:          I get that question a lot. One of the main areas that Sonatype has products is in this area that we help people understand all of the information about the components that they're using in their applications and be able to define via a rules based policy engine. What they think is okay and not okay. And because the repository is immutable, because it is so broadly used, we can't simply just decide that, hey, commons collections has a vulnerability in it in some cases. Whose determination is it that, that is universally bad for every single instance?

Brian Fox:          Because if we literally struck it from the repository overnight, potentially millions of builds would just simply often blow up. Many, many, many of those applications, probably the majority of them are not actually exploitable. I always use the analogy of book burning.

Brian Fox:          I mean, just because some people don't like a book does not mean it should not be available in the library for other people to look at. And that's really what it comes down to. So the information that we can provide to help people make better decisions is really the key way to do it. Simply just taking an artifact and making it disappear from the internet is not the right way to solve this problem.

Andrew Nesbitt:     And often I guess with all the compiled JARs, that's not actually going to remove it from people's applications. If you removed it from the repository with the things that they were already running in production.

Brian Fox:          That's right, because they should have a local repository manager that is already cached a copy of it. All of the Maven systems cache it locally on disc as well. And so even if we disappeared it from the internet, it's still going to show up in applications forever and worse, what about all the legacy applications that aren't actually being actively built? They're still going to be sitting out there running just as vulnerable.

Brian Fox:          And so that's why we believe sharing the information, providing the information, the inspection and the analysis via the tools, key area of the business. But book burning, not a good idea. Now clearly if there was something in the repository that was clearly malicious, that would be a different story. Outright malicious code, we can nuke that. I don't think anybody is going to be upset about that. But determining that a component is universally bad for everybody in all instances just doesn't make sense.

Andrew Nesbitt:     So who does get to make that call?

Brian Fox:          To make which call?

Andrew Nesbitt:     Whether something is universally malicious or just potentially exploitable?

Brian Fox:          Well, fortunately so far we haven't had to make that call that something was clearly malicious, but it would be somebody here at Sonatype maybe me asking an opinion. If it was not clear cut then we might consult, the community, the Maven PMC, things like that to get some buy in before we took some universal action. We always think about the community and making sure we're doing right by that and getting a gut check is always a good thing. But so far to our knowledge that is not happened and it certainly hasn't been a decision we've had to make yet.

Andrew Nesbitt:     Do you have fine grained analytics around the downloads for each artifact?

Brian Fox:          We do, through the various forges, like at Apache or the one that we provide. We provide those statistics back to the projects themselves so that they can see what the popularity is of their component, what the different versions are. It's also something that we weave into our various tools. But we do processing that, generating those with the amount of traffic we do is a little bit complicated.

Brian Fox:          Maven itself was the repository, like I said, it was designed to work well on a flat file system, which means interrogating all of the stuff and rolling that up and making heads or tails of it is not always easy, but it is possible. You end up doing a lot of file path manipulation and things like that to kind of roll up the statistics.

Andrew Nesbitt:     I think the Python project has been doing a little bit of that with Fastly where they're exporting their traffic logs in anonymized form to Google BigQuery and they have a public set of download logs where people can then go and investigate for particular traffic patterns or download activity on different kinds of modules. And I ask that mostly because, if you've got their information potentially that can help inform, is this malicious package heavily depended on and heavily downloaded or is it, does it look like no one has really realized it's there yet and no one would really be hurt in the act of removing it?

Brian Fox:          Right. And there was an attack on the Python PyPi recently. Well, it started earlier in the year with typosquatting on npm and then somebody did it on Python. Within the past couple of weeks I saw same type of thing. And so I got a bunch of questions from the community and from customers asking, what's my take on that? I think in this instance it kind of comes back to the lack of the namespace that I was talking about before.

Brian Fox:          It's really easy if you only have a single name, say Struts and you want to publish a pretend version of Struts, it's easy to typosquat that. But when you're actually looking for org.apache:struts, it's a lot harder to typosquat that. And because we don't just let anybody show up at Central and just push anything into their under any name, they have to basically ask to become a publisher. And the way that works is we then assign them a namespace.

Brian Fox:          A lot of projects are comgithub.projectname. And so we give them the ability to publish into comgithub, their project name. If somebody has their own domain org.fu, then we would give them the ability to publish components into org.fu. And so that's just sort of how Maven works, it's how Maven has always worked. And so while it's possible somebody could typosquat that, it makes it a lot harder because you have to get multiple mistakes piled up in the consumption.

Brian Fox:          And we also double check when somebody comes to sign up, we have a number of different ways that we look to make sure that they in fact control the domain or that the project is what they say it is. So we do the best we can to make sure, somebody is not masquerading. But if we didn't have the group ID as part of Maven, it would be that much harder. And that's one of those things that I really wish new package systems that came along would really think about that.

Andrew Nesbitt:     It feels like a lot of the similar problems to DNS were trying to decide on should this domain name be allowed where it matches, or it's very close to Google but it's using like a [inaudible 00:45:39] zero character instead of an O.

Brian Fox:          Yes. It's a very similar problem. I mean, in this case it's maybe, I guess it's maybe more like issuing an SSL certificate for a particular domain, making sure that the person asking for the certificate is authorized to secure that domain. I think that's sort of an equivalent to what we do, before we give you access to just universally publish into a particular ecosystem.

Andrew Nesbitt:     Where npm is more like let's encrypt and it allows you to self serve?

Brian Fox:          That's a great analogy. Yes, that's exactly it.

Andrew Nesbitt:     So Java as a mature project has a lot of different versions that are out in the wild that projects are using. How does Maven help people developing packages to manage packages that run on all those different versions?

Brian Fox:          Great question. For the most part, components built with one version of Java are forward compatible. So something built with Java 5 Java 6, will one run just fine on Java 9. And so dealing with the vagaries of Java itself, the run time is not something that Maven has to tackle. Where we get into that a little bit is, sometimes there are requirements that you can set in your POM that my project must be built on at least this version of Java, that so you know it's going to compile and that you can produce the byte code that you want.

Brian Fox:          So Maven the project itself, helps people deal with that. But there's not a lot that we have to do at least so far in terms of the Java versions. Now I mentioned earlier the Jigsaw project from Java 9 which is an attempt to modularize the JVM itself and it produces a first class concept of a module, which is very similar to a Maven dependency, but it's not quite. And so a lot of the work on within Maven lately is to try and figure out how do we maintain compatibility with this new model.

Brian Fox:          It really is sort of a shift between, the way the old Java versions wanted just everything on the class path. And so Maven just had to make sure that all the JARs were put into place and it was consistent and there weren't duplicates and things that, to now the new Java system, when things are using Java modules, it's a completely different paradigm that they have their own manifest, which sort of looks a POM, it declares what the object is, it declares what its dependencies are.

Brian Fox:          And so marrying those two things together is quite a challenge. And the thing that we were working with some of the spec leads that [inaudible 00:48:13] make sure that they weren't accidentally creating this shift between the old Java components and the new Java components that, that would in my opinion, kill the ecosystem that you basically can't start Java over these days. There's so much competition with all the new different languages that are out there.

Brian Fox:          Where Java's power comes from is in fact its heritage, that there are so many packages that are out there and all of them can be run on the modern system. And if we were to accidentally break that, that would be I think a death knell for the ecosystem. And so there was a lot of back and forth on that earlier in the year, but we ended up working through that. And I think it'll be a much better seamless transition as we move forward so that people don't have to modularize these old legacy JARs to use them in a new application, which was, what was potentially about to happen.

Andrew Nesbitt:     Interestingly we're seeing that happen in the JavaScript world right now where the browsers and IES 2016 is introducing its own modules back, which is not compatible with the, I want to say it's common JAS spec that npm works with. And what they've actually ended up doing as far as I've seen so far, and this may be still subject to change, is that the new module files actually have to have a different extension, which is .mjs rather than .js because when you do require inside of a node project, it has no idea whether or not it is IES four module or IES 2016 module and those two things act very differently because, basically the browsers have decided what they're going to do and know it has to change to make sure it continues to work with those things. But it means that they've basically kind of splitting it right down the middle. There's new types of packages and old types of packages, and trying to make those two things work and be backwards compatible is potentially really painful.

Brian Fox:          It's definitely a challenge. And as you were describing that I was thinking, Maven is somewhat unique in some of these ways that Maven doesn't control Java. Java and the JVM is, an Oracle/open-source project, and Maven is an open-source project. And so we can influence them but we don't control it when those things change. And conversely, [inaudible 00:50:38] wants to make some changes in npm, it's sort of much more tightly coupled together.

Andrew Nesbitt:     No did npm, but not necessarily the web browsers, which have a slightly different view, especially when it comes to loading things into modules and you're like, well, we don't want to download this humongous amount of things just for the web page. We want to be able to load them all into a single name space or at least flatten everything out as much as possible.

Brian Fox:          That's right, and that in that instance they don't control the runtime, and so they get whipsawed as well. And so sometimes you get caught in the middle where you're trying to maintain backwards and forwards compatibility at the same time. That's a uniquely package manager problem I think in some ways.

Andrew Nesbitt:     In one of our previous episodes we talked with [inaudible 00:51:23] who was one of the people who maintains CocoaPods, and CocoaPods is in this interesting situation where it's used to package macOS and iOS code, and Apple have released the swift package manager, which is only suitable for some use cases, but the writing is clearly on the wall for the CocoaPods project. At some point everyone is going to switch over to the swift package manager. Is this the same for Maven? Do these Jigsaw modules also indicate that Maven's days are numbered?

Brian Fox:          No. In fact, the Oracle team was very clear that their role in this was not to try and replace the build systems. That final package and assembly often in a compilation of an application was clearly not something they were trying to take over. But instead they were trying to modernize the system to introduce things that the LSGI project has done for years.

Brian Fox:          There's many different modular systems that have built on top of Java and they all have their own idiosyncrasies and I think they were trying to provide some of those aspects so that they could lighten up the actual JVM itself so that, if you had an application that didn't need all of the different functionality that it only fetched the core modules required. So no, that there definitely isn't that not on the horizon for Java.

Brian Fox:          On the CocoaPods side, I did some research as I was looking into it, some of our customers from the Nexus repository manager side want us to support, all these different formats that on the repo manager side we're also in this weird situation where we have to almost be universally accepted for all these different ecosystems as well as some of the component management and the security stuff we talked about before.

Brian Fox:          And I was surprised how much swift open-source components are still out there and distributed by CocoaPods. So, yes, a default package manager from Apple makes things a little bit easier. I'm not sure that means the death knell to CocoaPods though. I think it's an for them to go beyond and provide a better experience functionality wise. They'll obviously have to maintain compatibility with whatever happens in the package.swift files, but I'm not sure it necessarily means that a death knell for them.

Andrew Nesbitt:     The maintainers seem pretty convinced that not imminently, but certainly on a five to 10 year time scale. Then things are moving in a swift package manager direction.

Brian Fox:          Anything can happen in 10 years, for certain. But fighting the default effect is definitely an uphill battle, there's no question about that.

Andrew Nesbitt:     Another aspect which kind of plays into that is Apple from all of their wonderful qualities are not known for their open-source work and being engaged with community projects. And I wouldn't particularly put Oracle up there either myself, but I'm guessing you found them easier to work with. What is that working relationship like between Oracle and the Maven project?

Brian Fox:          I think there was some resistance at first to understanding where we're coming from. I think we were coming from very different angles and it took a little while to realize in some cases we were just vehemently agreeing, and in other cases we were just speaking a different language. In the history of Maven, we haven't had a lot of cross pollination between the core Java guys and the spec leads. But they were off doing their thing and largely we were doing our own thing.

Brian Fox:          This was the first time where we really sort of had this massive overlap in domain expertise, and so it was a little bit bumpy, but they kind of took a lot of the data that we were providing from the Central statistics. We were able to provide a lot of the data on what were the most popular components and how were their usage patterns related to other ones. And basically giving them a better picture of what the typical library would look like, and how frequently it's updated and how many different versions are in compatibilities and lots of stuff like that.

Brian Fox:          In the beginning it seemed like they weren't listening. I think it was actually, they were just being very thoughtful. And so in the end, the first version of the Java spec didn't pass the vote. And so it was a little bit of a scramble to go back and try to appease the community and get some of those things fixed. And they definitely, they involved us. There were two of us from the Maven project that were literally on video conferences with them multiple times in a week as we worked through different issues.

Brian Fox:          So in the end they were very open. I think they did a great job. Mark Ryan hold was the spec lead and I really commend them on bringing that to ground and finally getting the spec out. I mean, I can remember 2007, 2008 when it was first announced that they were doing this Jigsaw thing, and we were very early days with Sonatype and we were very much doing a lot of Maven stuff back then. And I can remember feeling somewhat threatened by the Jigsaw like, no, is this gonna kill Maven? Is it going to break everything? Is it going replace it? It took them a long time to get it done. But it's finally here and all of my fears didn't come to pass.

Andrew Nesbitt:     Am I right in thinking that in Java you can only have one version of a module loaded at once?

Brian Fox:          In the old version, in a class path, yes, that's true. Because what literally happens when it's trying to find the class, it will look through all the JARs in the order that they appear in the class path. So more advanced systems like LSGI and stuff they kind of mess with the default class loading and they can do some stuff to allow you to have different versions running at the same time. But they really have to do a little bit of black magic.

Brian Fox:          In the Java 9, the Jigsaw stuff, I believe it is possible to actually have different versions. And that's part of the trick is how do you take something that was designed to work in a class path and make the legacy aspect of that still work in a new system that is trying to do class path isolation and module isolation? That was really the challenge, and the devil is in the details in how effective that can be.

Andrew Nesbitt:     And the potential pitfalls that come with that where especially in an object oriented language, the object's generated by those different versions of that same module could actually be incompatible with each other.

Brian Fox:          I mean, what do you end up having to do is build isolation from the top down. So if you have a dependency that needs version 1.0 of a logger and then you've got some other dependency that needs 1.1 of the logger, you need to make sure that, when they make their calls, it's loading it in the scope. And so that's where all of that isolation comes to pass. And that's a little bit outside of my domain expertise.

Brian Fox:          I've poked at OSGI and some of those types of things that were doing similar stuff and it's a little bit like black magic. But it's a really hard problem to solve because that 1.0 of the logger may itself want, 50 other dependencies and the 1.1 may have, 48 of them are the same and two of them are yet a different version.

Andrew Nesbitt:     The dependency resolution suddenly gets either potentially much more complicated or you go the way that a classic MBM worked and don't even try and [inaudible 00:58:41] all of those things and just allow each version of each dependency to have its own set of dependencies and end up downloading every version of everything.

Brian Fox:          Right, and Maven, that's one of those things that may have to be altered as things go forward. But Maven always did that version resolution for you because it knew inherently that you could only have one version of a JAR or at least Maven would enforce it. Other tools may not, and then you get really unspecified behavior at runtime. And so Maven would be smart enough to recognize that, there's this transitive dependency version conflicts with that other one. And so there were [inaudible 00:59:20] for to how to manage that.

Brian Fox:          Not always in a way that made runtime work, but at least it would make sure that you had one version and if it broke a dependency or it was unresolvable it would tell you about it. But if we have to start supporting modules that have potentially conflicting versions, that behavior will have to change obviously.

Andrew Nesbitt:     So Maven is a long running project, but also you are looking towards the future. What are the things that you're currently working on and thinking about? What's coming up in the near future and slightly less near future for Maven and Maven Central?

Brian Fox:          I think some of the stuff that has to be considered is making Maven better suited for a continuous delivery type of pipeline. In the early days, some of the standards of best practices were baked quite literally into how artifacts were built in that, we had this concept called snapshots and then releases. And so the expectation there was that every time you ran a build, say in a CI, a timestamped version of that 1.0- some long time stamp would be pushed into the repository.

Brian Fox:          And builds that were consuming snapshots would have a different consumption pattern. They were, typically they would check at least once a day for a new snapshot. But you could change that to every builder every hour. And so that's how you would adopt more of a continuous integration strategy. But then when you went to cut 1.0, you would drop the snapshot part and now that's considered from Maven to be a release.

Brian Fox:          And so if you're looking for 1.0, and I have 1.0, it's never going to check the repository again for a new version of that. That's that immutability concept that I was describing before. But when you start to think about continuous delivery, you almost want to have some traits of snapshots in that, every time you publish a new version of it, you don't want to have to run around and change all the consumers to say, now I need 1.1, now I need 1.2, because they are specifying that in their dependency list.

Brian Fox:          And so the snapshot was meant to be that way that I could say, I'm depending on this mutable version of it. But there is a time where it becomes immutable itself. If you start to think about continuous delivery, a developer checks in code, it gets built, it runs through some tests, and then it's in production. How do you maintain the configuration management sanity that comes with immutable and concrete versions, but have the flexibility that snapshots and other things provide?

Brian Fox:          That's an area of continuous evolution. Being able to speed up the builds and do things in parallel. Also obviously a key thing as computers become more powerful and we can do multi-threaded compilation, but making sure that things come back together when they're supposed to be obviously areas of evolution. And then some of the stuff I was talking about before of trying to figure out how do we break away from that 4.0.0 POM object model that's been in place since 2004, 2005 in a way that maintains backwards and forwards compatibility with all these other tools, but that, allows Maven itself to continue to innovate the way it does things.

Brian Fox:          So those are always ongoing areas for the Maven project to tackle. The Jigsaw stuff, Java 9, are whole new world. There's a lot of work that has been done by a few key people that are, making sure that we're compatible and as people start to adopt that, certainly there's going to be lots of new use cases that come up that will drive lots of new either core changes or just new plugins that do different things.

Andrew Nesbitt:     And what about other package managers? Do you look around at any other projects, maybe newer projects with envy, or are there things in there that you wish you had available to you in Maven?

Brian Fox:          Unfortunately, I more often look at them and go, why? Why didn't you follow some of the paths that were laid out before? I feel like a lot of the rush to produce yet another package manager and to do something new and novel accidentally introduces new challenges like the name spacing that I was talking about and some of those other things. The GO ecosystem is one that we're watching closely because that seems to be taking off, tremendously.

Brian Fox:          But that launched without a standard package manager, which was I think the first time that's really happened in a long time. And so in the early days of GO there was like 20 different competing package managers, and if you were trying to get into that ecosystem, how would you even know where to start? So it's great to see that they're starting to put together a more of a standard package system for that, and I think that will really help that ecosystem take off.

Brian Fox:          But there's not a specific feature that I see that I wish Maven had. I think it's actually the opposite. I wish they considered mutability and how to maintain the Central repository with an eye towards the future versus, what's neat and fun right now.

Andrew Nesbitt:     Talking of neat and fun right now and immutability, have you considered trying to put Maven on the blockchain or on IPFS some kind of truly immutable fire system?

Brian Fox:          We get questions about the blockchain quite a bit. In the early Sonatype days, we were looking at chain of custody and chain of trust and some of those types of things. But it's not quite clear to me how that all fits together yet. Something that I ponder, on long bike rides but not something that we've started to move on yet.

Andrew Nesbitt:     So interestingly one of the reasons we started this podcast was actually to try and spread the knowledge around of different reasons and histories behind package managers so that for people who come to birth their package manager in the future can hopefully avoid some of the problems that you mentioned by learning from previous package manager builders. Because there isn't really that much kind of shared knowledge around it.

Andrew Nesbitt:     There aren't that many people who have been in the trenches building package managers and kind of saying, well, if I did this again, I would avoid doing that to try and help future people in the same way that crypto, a lot of the times kind of you're told, don't write your own crypto libraries. So there's very few people who know how to write a crypto library.

Brian Fox:          Yes. I think that's great. Can we make that maybe standard listening for anybody if you want to make a package manager, you better listen to all these? I know at one of our product meetups a couple of years ago, our repo team who like I said before, is sort of tasked with, they basically have to support all these new formats because that's what our customers want. It's not just about Maven anymore, it's docker and npm and Bower and Yarn and you name it.

Brian Fox:          And they're constantly frustrated with, because each one is a unique snowflake. And we were contemplating putting together a series of documents and or a blog. I'm not sure it ever saw the light of day, but we did start collecting the list and it was sort of along the lines of, so you want to write a package manager. And so if you're going to do that, or at least a repository format, here's the top 10 things to please consider it to make everybody's life better. That sounds like what you're talking about here.

Andrew Nesbitt:     So if people wanted to learn more about Maven and Maven Central and Sonatype, where's the best place online for them to do that?

Brian Fox:          Well, you can read about Maven at maven.apache.org. You can read the Maven book. It happens to be one that many of us at Sonatype wrote and provide for free. Back in the day when you bought printed books, you could find it in the store, but you can get it online. And Sonatype you can find out all the stuff that we're doing, in terms of caching proxies, and the repo managers, and secure component supply chains at sonatype.com.

Andrew Nesbitt:     And if people wanted to get involved with some of the maintenance or development of Maven, where should they look for info on that?

Brian Fox:          At the Maven project. It's sort of grab a shovel and start helping. A lot of the Apache stuff is available in GitHub now. So the modern tooling, the pickup and submit up a request.

Andrew Nesbitt:     Well, thank you so much for coming on and talking to us about Maven and Maven Central and all of the experiences that you've had over the past 13, 14 years working on this project. It sounds like you overcome a lot of tricky situations and learned a lot about package management.

Brian Fox:          Thanks for having me. It's been great talking to you guys.

Andrew Nesbitt:     And that's what we've got for this week. Come back in two weeks time when we'll have a new artifact published.
