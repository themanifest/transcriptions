Episode 2 - CocoaPods with Orta Therox - https://manifest.fm/2
--------------------------------------------------------------

Andrew Nesbitt:     Welcome to the Manifest, A podcast that's all about package management. I'm Andrew Nesbitt.

Alex Pounds:        And I'm Alex Pounds.

Andrew Nesbitt:     And together, we're going to explore the technical details of package management, the stories of the people behind the projects, and the communities around too. Today, we're joined by Orta Therox, an engineer at Artsy, the creator of Danger, and one of the main maintainers of CocoaPods. Orta, welcome to the Manifest.

Orta Therox:        Hey, thanks for having me.

Andrew Nesbitt:     So first of all, I think we should get started with what is CocoaPods?

Orta Therox:        So, CocoaPods is a dependency manager for Mac and IOS projects. So, it's only really specifically for all things that are run by Apple tooling, and it does this by generating native code projects and then sort of building that into Apple's existing tooling infrastructure and subsequently, every time Apple's existing tooling infrastructure changes, we have to keep things up to date.

Andrew Nesbitt:     So, it's not an official project supported by Apple?

Orta Therox:        No, definitely not. It was a project that a friend of mine called LA Darren credit back in 2011 that I joined maybe in 2012. Having any dependencies inside native code is difficult. There's a reason why like C++ doesn't have a dependency manager. So, what my friend was finding was that trying to just update a single dependency inside his project was taking a very long time. So he took a lot of the ideas from Ruby Gems and Bundler, which I think was pretty new at the time, and try to reimplement those, but inside a native context.

Alex Pounds:        And when we talk native context, are we talking just objective C or Swift today?

Orta Therox:        Yes, today Swift. But you know, six years ago, objective C was the only thing that existed in terms of building native tools, but CocoaPods supports everything that Apple supports. So, objective C and Swift mainly.

Alex Pounds:        So, before CocoaPods existed, how were people managing or sharing libraries between projects?

Orta Therox:        CocoaPods came and said that we should have a kind of manifest file that describes how the library should be linked to a project, all the different source files and resources that should be in there. Before then, you would literally just have a list of instructions which is download this zip file, drag this source file into this folder, drag this one into another one. These resources need to be put in these specific places.

Orta Therox:        You would then have to have like individually upgrade recommendations for every time you had a new version, because you know, if a file had been deleted then you had to download the new zip, and then you have to remove the old ones from the project, and it was most definitely a mess. But for a lot of the culture and native development, that's very much a not invented here kind of feeling.

Orta Therox:        So, the idea of including dependencies wasn't very big back in 2011 just in general, because at least for IOS, people were not making very big projects. On the Mac on the other hand, it was very common to make these kind of mega frameworks that people could use. They'd probably feel a lot like rails does on the website where you would have one big monolithic thing that you drag in.

Orta Therox:        And then while for IOS it was just kind of free for all drag and drops, change these settings and eventually CocoaPods tried to turn that into something that was scriptable at different levels, so that you could kind of handle something as crazy as like open SSL, which is like a big C++ library, but at the same time handling objective of C networking library and use the same system.

Alex Pounds:        So, CocoaPods came out in 2011, and was a roaring success and is still going strong today.

Orta Therox:        Yeah, it's definitely the dominant package manager in the native ecosystem. It would be pretty surprising to see libraries coming out today that don't have CocoaPods's support. The alternatives are not quite as comprehensive as CocoaPods either in terms of like not supporting many of the features, or having pretty drastic trade-offs in terms of long-term upgradability for your dependencies.

Alex Pounds:        And how did you get into programming to begin with?

Orta Therox:        So, I was a programmer because I wanted to make games. It's like such a stereotype. I did that for a while, and I had my first job out in Brazil actually making iPhone games. Once I came back from the country because I got kicked out because my visa expired, I ended up going and doing a free month hacker school on design.

Orta Therox:        And that's actually probably been one of the most useful fundamental tools that I've had in terms of being a programmer in open source. Because, a lot of the aspects that I'm interested in as a developer are around documentation, community management, trying to help people help themselves. It gets really hard to build very big projects with very few people.

Orta Therox:        And I think improving documentation is usually one of the first easy steps to reducing workload on individuals and letting them focus on what they really want to be doing. And so, a lot of my training around design has come really in handy in both being able to build an entire product myself, and to be able to say like, "I feel like we need to make documentation improvements in these different levels of the project so that we can actually not have to get as many incoming requests."

Orta Therox:        Being able to keep track of issues and finding out why people are having the same problems, makes it easier to understand where the root causes of those are.

Alex Pounds:        What kind of games did you want to make?

Orta Therox:        Back then, I started by copying metal gear solid and trying to make it 2D because, you know, that was the cool game at the time. But the only real game I've ever actually published is a finger hockey game. So like air hockey, it turns out the AI is actually the hardest part of making a game sometimes. So like the single player mode.

Orta Therox:        I ended up making all sorts of different algorithms for figuring out how to give you an interesting character to play against. But at the same time, like be fair, and it realistically is really hard.

Andrew Nesbitt:     And how did you get involved in CocoaPods?

Orta Therox:        My style has been very interesting because the creator of CocoaPods alloy is very much a pragmatic person. He'll create something that will work in a small amount of code as possible in order to get something usable as quickly as possible. So, instead of like a package registry, it created an idea called the specs repo, a singular get repo where a collection of recipes exist for every single library.

Orta Therox:        And so, what I did to start contributing was I just started reviewing poll requests to that one single repo, which I did for about a year. Every single upgrade of every single library had to send a poll request to this one single place. So, it was just looking at these dependency changes saying wherever they'll break or not, and whether it'll continue to work in the future.

Orta Therox:        And it gave me a good chance to say hello to a large amount of people whose work I actually relied on. And then after doing that for maybe a year, I started feeling some problems around documentation in the community as it was only just started to centralize on the idea of CocoaPods. I started to build something that's very similar if you're using ruby to ruby duck, unlike a ruby where it's quite easy to pass their source code at run time and pull out documentation of any class.

Orta Therox:        In order to do it on a native code base, you actually have to compile the entire project. And that's time consuming to say the least. I started building this system that would statically compile every single library in the ecosystem and generate documentation for them.

Orta Therox:        And so from that, I ended up building that into the CocoaPods website, and eventually just sort of became someone that was very involved in a lot of decision making for long-term perspective, and not necessarily being the person that writes the code to make something, but being someone that's involved in the discussions about whether that's something we should build.

Alex Pounds:        So, you mentioned before that CocoaPods has a rosy outlook and everything is great with it. But maybe there's a little bit more going on these days.

Orta Therox:        So, the interesting stuff about CocoaPods in my opinion, is that there is a thing called Swift package manager. So, for the rest of the world, I'll give you the background. So, CocoaPods has been around since 2011 and somewhere around 2015 Apple announced that they are creating their own package manager. This package manager is built entirely for Swift inside Swift and will be shipped with every single set of the Apple tool chain.

Orta Therox:        So, it will be included on everybody's Mac books and inside X code. So by default, it's there. And that's created a really interesting problem for someone that's been running a package manager for the last six years. It's actually this really interesting problem of like, "Apple should be doing this. Apple should have been doing this from day one realistically."

Orta Therox:        Because, a package manager is like an essential tool in any ecosystem. And the fact that there literally wasn't one and the Apple effectively discouraging use of one was really inhibiting the community. And they ended up making like a split in the community in terms of like there are people that believe that Apple's way of saying you shouldn't have dependencies is the way.

Orta Therox:        And then there's everyone else that's come from every other ecosystem that's like, "Well, I want to share and reuse my code with other people. I want to share and reuse my code amongst many apps." And the monolithic way that Apple would like you to be building your apps is not quite the only way in which you can build things. By releasing Swift, Apple started targeting Linux for the first time.

Orta Therox:        So, Swift package manager only works realistically on a server side Swift kind of project. You can't use it in IOS or Mac. And it's really aimed at people that are trained to do so besides Swift or like command line tools, which is effectively a very niche market.

Alex Pounds:        Why do you think Apple made that decision? Why did they decide to target it just to that services use case?

Orta Therox:        I think Apple target its server side Swift as their first use case because it's a very small, understandable use case for them. CocoaPods took five years to become a 1.0 product simply because there was so many things that need to be covered. Like building libraries for IOS and Mac projects require building ginormous tool chains that have tons and tons of features that need to be covered.

Orta Therox:        Or as just for a server side, Swift, all you're effectively doing is building an executable. So, you can lean on LLVM very heavily inside Swift package manager, which is what I'm pretty sure they do. Whereas on the CocoaPods side, what we tend to do is lean on Xcode. So, LLVM is the compiler and X code is like the ID that sits above it.

Orta Therox:        Both of those are like different levels of abstraction, so we as CocoaPods have to understand that like the LLVM level a little bit, but we don't have to understand it in the same way that we'd have to understand how you'd create a project or structure your tooling inside X code. So, it's much easier. It's a great place to get started. They can start with a very fresh perspective because nobody's ever done Swift on the server before.

Orta Therox:        They don't have the problem of like breaking changes when suddenly the manifest file doesn't work because they've decided a different version, a different way to describe the problem of a library. If they were doing that for millions of users, I think it'd be much harder to do than if they were doing it for under 10,000 users in terms of people probably using Swift on the server.

Alex Pounds:        So, it's kind of like the beater version of the package manager.

Orta Therox:        Yeah. Apple are really taking their time with this, and I mean that in the sense of like they're trying to do it really right. They're taking it like a very serious software project that's like LLVM level of quality. It's not the equivalent of what CocoaPods was after its first two years, which was used in tens of thousands of applications probably by that point.

Orta Therox:        But it was also just like quickly done to do the minimum required to get everything working, or as Swift package manager feels like they're really trying to build foundations that could last for decades. And so, it's taking time to get it all right and building it into every single useful pit of infrastructure. And then, they'll probably start looking at how do we work it into the large set of projects that are IOS apps and Mac apps?

Alex Pounds:        Have you ever had Apple reach out to the CocoaPods team to actually kind of work with you directly?

Orta Therox:        So in this case, yeah. There's an annual developer conference that Apple runs called WWDC, and for almost every year for the past five, we've had a CocoaPods meets X code team meet up where it's usually, it's just like we do dinner together and just talk about what they've released this week and how they've broken everything for us. And that line is where we can continue to be supporting all the features that they want as well as trying to think in terms of long-term.

Orta Therox:        We want people to be using CocoaPods for a long time because generally, we want stability in our tooling and ideally, we want to find the smallest change as possible for everybody's project in order to it to continue running. But on the positive side for Swift package manager, someone that has been working on CocoaPods for two or three years is now working full time inside the Apple developer tools team.

Orta Therox:        And if I had to guess, it would be somewhere around this kind of problem, but I don't specifically know what he works on and I wouldn't pry anyway because Apple renowned for their secrecy.

Alex Pounds:        Has that secrecy ever impacted upon CocoaPods?

Orta Therox:        I mean, it impacts every single year. Every WWDC, something will break with CocoaPods because a new version of X code is released, which has different kind of foundations to work on. For example, this year there was the idea that instead of one version of Swift existing inside the compiler, there's now two versions of Swift.

Orta Therox:        Different modules inside a single project can have different versions of Swift running on them. So like the compiler could be version three, or version four, and each individual small chunk of code could turn into a single project that could be compounded by multiple bits.

Orta Therox:        So suddenly now, CocoaPods has to be able to deal with those kinds of things, and obviously, people want to use those features, so CocoaPods has to support those features. In this cases, it wasn't like that breaks the today, but sometimes it does.

Alex Pounds:        And do Apple ever donate any development time or any way of financially supporting the project?

Orta Therox:        No. No support at all. Aside from that one single dinner, that's basically about it. I mean, I don't know what it's like internally. So, I always like to try and assume the best in that the team that looks on X code will test the CocoaPods project every so often to make sure they're not drastically breaking everything.

Orta Therox:        Because, there's like millions of projects using this, so if they make something that drastically breaks CocoaPods, then they're drastically breaking millions of people's projects. So, I'm pretty sure they're testing that kind of stuff internally and trying to not be drastically breaking, but just like the minimum amount of breakage they need in order to get whatever things that they want to support.

Alex Pounds:        So, you're obviously familiar with the internals of CocoaPods and will have seen some of the internal details of some of the Swift package manager. What are the key differences between those two systems? What are the decisions that you've made differently to the Swift package manager?

Orta Therox:        That's a really great question. Now I'm not actually too sure of the exact answer. The biggest fundamental difference is that CocoaPods is built in Ruby and Swift package manager is built in Swift. It uses the word pins instead of look for a locked file. And it uses JSON for that instead of yellow. A lot of the ideas are quite similar.

Alex Pounds:        Do you think there's an advantage to having a package manager written in the native language for the system where it's managing those dependencies?

Orta Therox:        Yes, but I don't think it was a good idea for CocoaPods to have been built in objective C back in 2011. Coolbot was definitely in the right position for the time. And Swift didn't exist until 2014 maybe, but the biggest downside actually in my opinion, is trying to get people involved and trying to get people to stay involved is significantly harder when you have a different language for building the tooling than for the actual use case of that tooling.

Orta Therox:        We find that people end up getting good at working on CocoaPods and then they ended up being hired for projects that rely on building that kind of tooling because it's way more valuable than being someone that actually would use CocoaPods in Swift or objective C, and then they never need to use the project so they move on.

Alex Pounds:        One thing that both CocoaPods and Swift do, which I'm not a big fan of, is actually having a touring complete manifest file where you're describing your dependencies actually can be changed at the point where you want to read the list of the dependencies which in the case of CocoaPods, I guess is because they copied the Ruby Gems setup where the gem file can have kind of execution with ruby code anywhere in it, which means that you could actually add a new dependency on Tuesdays if you wanted to.

Orta Therox:        Yep. That's totally true. And like in CocoaPods, that's actually kind of useful because a lot of people have pm post install scripts that end up changing small parts of the project. But generally, yes, I know. Yeah. It's because CocoaPods is a third party tool that doesn't get native integration.

Orta Therox:        So, you have to tweak all of these parts of the system because CocoaPods shouldn't be doing some of these bits for you. I totally agree. I see the advantages and the disadvantages of both for both the manifest file and the manifest for a library because they're separate in CocoaPods just like Ruby.

Alex Pounds:        Not all of our listeners may realize why that this chewing complete nature makes Andrew's life a little bit more difficult. Andrew, why is this a pain for you?

Andrew Nesbitt:     The project that I work on, libraries IO tries to index every dependency in every open source library, which includes indexing each manifest file from every repository they can find. Those manifest files list out all of the dependencies, but if they're listed in a file which needs to be executed to get back the list of dependencies.

Andrew Nesbitt:     So I need to actually run the Swift code to find out what the list of dependencies are. That is for me, very annoying because I can't just pass it as if it was a yammel file or a JSON File, but also the security implications of downloading and arbitrary executable file from the Internet and running it blindly are, for me, doing it at a large scale, quite Dangerous.

Andrew Nesbitt:     But for individual developers, they're also quite a big security hole from the point of I'm just going to install a package by its name, and in running that command, I'm going to execute some arbitrary code that I've probably not looked at. The security side of that actually starts to bring in all kinds of interesting issues that are often kind of skipped over and seen as this is a feature rather than as a security issue.

Alex Pounds:        So, security and trustworthiness is really a big theme in a lot of package managers. Orta, does CocoaPods have anything to help developers trust the people whose software they're using?

Orta Therox:        So the registry for CocoaPods only holds JSON files. So in the case of most people's projects, the only arbitrary evaluation they've got is the manifest of all the dependencies that they'll want for a project rather than for each library. And I think it's always about trying to find a balance between convenience and extensibility.

Orta Therox:        CocoaPods has some really interesting attacks of this problem that is probably quite different from a package managers' because we're native developers. We have a sandboxed version of CocoaPods called Pod-sandbox where the actual executable of CocoaPods, so the bit that would eventually run ruby code is sandboxed to one specific folders.

Orta Therox:        So, it can only make changes inside its cash folder and inside a specific folder inside your project, which definitely mitigates some of this stuff, but it's obviously not perfect and we've never pushed it too hard as being the feature for every single person because we just keep finding edge cases.

Alex Pounds:        So, one difference between the Swift package manager and CocoaPods I see is the Swift package manager doesn't have any concept of a registry where it will just say point at an arbitrary get hub or get repository somewhere on the internet, which makes it really hard to kind of get a good picture of what people are installing and actually to find pieces of code. Have you found that the CocoaPods registry has been helpful in catching bad actors?

Orta Therox:        Realistically, we've never had any bad actors I can think of. Again, we're a pretty small community in comparison to every other dependency manager. So potentially, it's just that maybe we've not seen it or that it's still remaining well hidden. But I think that the idea of centralization is a real positive and it's pretty obvious that Apple would probably ended up getting into the registry thing eventually.

Orta Therox:        It was definitely on their list of to do's when they first released their Swift package manager and saying, "This is what we're going to do over time," because how else are you going to promote things that you like? I'm pretty sure the Apple wants to have some sort of verified libraries.

Orta Therox:        The only way you can do that is really by having a centralized space for them, maybe not in the same way that we think of a registry that you make a ping to an API. But not in the sense of like, this is developers to Apple.com/swiftlibraries, and that's why they highlight all of the things that they prefer to showcase.

Alex Pounds:        Now that Apple have got into the package management game with their Swift package manager. Have you had any feedback or communication with people in Apple about what they see the future is? Do they see CocoaPods and Swift package manager existing side by side?

Orta Therox:        I don't think so. In the sense of, I mean I don't really think they see CocoaPods and Swift package manager living side by side. Apple is definitely about having a controlled ecosystems that they can set a high barrier of quality on everything. CocoaPods is completely outside of their control and I'm pretty sure they would rather own the entire developer experience for building IOS and Mac apps. So, it's very reasonable to think that they would like CocoaPods deprecated as soon as possible.

Alex Pounds:        Is it possible to manage objective C and C libraries with the Swift package manager?

Orta Therox:        Yeah, I'm sure. You can definitely do objective C and C++ projects with Swift package manager. So, they call it Swift packet manager, but Swift still relies on a lot of C libraries. So, they still need to support a lot of those in order to build their own tools, in order to not have to rewrite something like open SSL, you realistically have to support those kind of tools.

Alex Pounds:        So realistically CocoaPods is been deprecated from Apple's point of view.

Orta Therox:        Yeah. Realistically that was about two years ago. You know, the CocoaPods team has never sat there and said, "Well, we're gonna fight this and we're going to make the best product we could ever do." because that's very much an uphill battle that we'll never win. So, straight after Swift package manager came out, we started saying, "Well, we need to figure out what 1.0 looks like for CocoaPods."

Orta Therox:        Because, by that time I think around like .38, and like we'd always been saying like, "CocoaPods is production ready, but at the same time, because X code changes every year, but we don't really know wherever we can make you a promise that this is not going to be non-breaking every single vision." So we tried to figure out every single thing that we wanted to do that would have a very long lasting effect.

Orta Therox:        We called that a version one. That was a matter of choosing the features, trying to build things so that they will last. We built something called the CocoaPods Mac app, which was an attempt to fix the problem of all these developers have never used the terminal before and only use it for CocoaPods because they're native developers, and they're used to native tools.

Orta Therox:        And it was really useful because people that don't use terminal don't know how to debug a ruby stack trace or understand why a command isn't working. And so, we managed to cook an entire swath of developer problems. So, we'd focused a lot on trying to build something that was a bit like a turtle so it could withstand the time because it was obvious since CocoaPods is now deprecated, there's much less reason for us to spend our own time on it.

Orta Therox:        Why buffet building new awesome features when you know that there's just of Damocles above your head? And so, I eventually slowed down my own contributions. So did almost every major contributor to the project, and we just started thinking about how can we make these things last longer?

Alex Pounds:        I guess the flip side of all of that is that even though the writing is on the wall for CocoaPods, so they still tens of thousands of apps out there which are using CocoaPods and they're still relying on it.

Orta Therox:        Yeah, millions. Hopefully those people will be contributing fixes and keeping everything running just like the maintainers are doing now. It's getting new features, but it's getting new features slowly and it's been done extremely conservatively. That's really the change of the last two years. It's been whenever new things are added, there's a lot more discussion about wherever we need it or whatever it should have it.

Orta Therox:        However, they actually introduced this instability into the ecosystem because a lot of what we're building now is about trying to make sure it's going to last for a very long time. And that matters in terms of like how much money it costs to build services on top of CocoaPods? How much time does it take to maintain the web search engine or ensuring the SSL certs always up to date, and all these other things that they take an individual's time, but what if that individual isn't giving time to that project anymore?

Orta Therox:        It's a problem most dependency managers have of trying to get new people in, but it also has the problem of it doesn't actually feel worth your time even though all these people are using it still.

Alex Pounds:        Have you seen any projects that you think sun-setted themselves very well? Or ones which you've seen you thought did a bad job of that?

Orta Therox:        So, Ruby Gems felt like it was sunset for a while back in like 2008, nine. Well, it just went down to just like one maintainer going along, chugging along before ever being together eventually brought it back to life and started working on it again. I think they did a good job of battening down the hatches and just keeping the project running even though it was being used everywhere, but being maintained by a very few people.

Orta Therox:        But I realistically can't think of any of the dependency managers that have ever been deprecated since I've not really been able to take any useful advice out to find anything useful for me. Do you guys have any ideas?

Alex Pounds:        So, I've seen a couple of a package managers actually kind of been turned off and both of them were just instantly turned off. The servers were shut down, and it was not a pretty sight. One of them was the jam package manager, I think it was based off of MPM, but designed for the browser back before Bauer was a thing and really when MPM was getting started.

Alex Pounds:        But it just didn't get a lot of usage. MPM took over and one day, the maintainer decided just to turn off the server, which of course killed it straight away and people naturally had to swap if they wanted to continue to use their things. At least it kind of was close to MPM in terms of usage. I guess you could potentially provide some way of turning your pod file into a Swift file for people to be able to migrate onto the next package manager for them.

Orta Therox:        Yeah, I think a lot of it is that our infrastructure is built for a long-term, so even if ... it would take get hub going down for CocoaPods to stop working for most people right now because we're not running our own registry, we're not running servers that are actually critical to the path of using CocoaPods. If I run out of Roku credits tomorrow, all the CocoaPods website was go down, everyone's projects will still run. It's been a trade off on some of those decisions, but at least we've done the right thing. We've been able to make sure things continue to work.

Alex Pounds:        So, you don't have much in the way of material costs for actual services being run at the moment?

Orta Therox:        Yes. Basically I'm pretty sure CocoaPods is in the range of $500 a month for every single piece of infrastructure.

Alex Pounds:        Is Artsy picking up the bill for that or is someone else paying?

Orta Therox:        Some of the actual services are hosted. So, we have a coupon for AWS. We have deals with Roku, there's a company called Button that help out with some of my hosted Mac Minis service, and Artsy will just cover anything that kind of fits in between those gaps. So, no individual maintainer is effectively getting hit by these bills, so I assume they will be continuing until we find the way to turn the lights off. But, I don't think we'll be turning the lights off in any real sense. It might be like a decade when the CocoaPods website will get so little traffic that it'll just not be worth it.

Alex Pounds:        Even if X code has broken the integration with CocoaPods within a couple of years?

Orta Therox:        I think it's reasonable not to assume that somebody will be fixing those because people will still be using it. But that's just like somebody understanding [inaudible 00:30:33] project and making a small patch here and there, and as long as there's still someone around that's got the credentials to ship the Ruby Gems version, I don't think it should be a problem. There are big names that use CocoaPods. And so, it's very reasonable to expect a pull request fixing some arbitrary X code change insight for X code 10 in a year's time.

Alex Pounds:        So one thing you said before was that when CocoaPods was set up, there wasn't quite such a culture of open source and using library's among developers in IOS. Have you seen that change over the years?

Orta Therox:        Yeah, definitely. And especially in new people to the community of building native apps. It's a weird pitch. But CocoaPods really lowered the barrier of entry for people that want to make IOS apps simply by making it easy for you to reuse infrastructure that other people have built in terms of networking libraries, being able to copy the kind of views that the visuals of applications quite easily. So, it was a lot easier to get something started.

Orta Therox:        So, there was a lot of criticism in the community of all these people that just use CocoaPods to build their apps, which is very similar to if you imagine someone saying that, "People using node are I just building their own little applications out of bricks of node modules and not knowing the entire infrastructure."

Orta Therox:        I think it's a trade off of you do want to get started somewhere and if you can reuse other people's work and just understand that that abstraction level, then it really does lower the barrier to entry at the trade off of there's loads of people and now that don't actually understand the stack that they're sitting on

Alex Pounds:        And they'd be doing that with Apples sense of libraries as well?

Orta Therox:        Definitely. You can't see the source code for Apples library. So, you're one up on that at least, but Apple will be documenting it well and maintaining it and having support avenues that you don't necessarily get with open source.

Alex Pounds:        Personally, I think that's one of the strengths really of any open source software, that ability to stand on the shoulders of other people who have worked on things and build things before you. I remember I saw in my own development kind of a shift, and it was probably around 2014 where I used to be the kind of programmer who would really want to build everything myself.

Alex Pounds:        Like of course I'm going to build my own login system. It's just a couple of cookies, and a couple of forms, so where's the harm? But now I'm much happier using device which is going to do a lot of that stuff out of the box for the ready web projects which I'm working on. How do you feel about that kind of package based development? You think it's a good thing for the industry?

Orta Therox:        I personally do. I believe that everybody should be able to build what they want to build, and they should be able to choose what levels of abstraction that they want to actually work out. And I think relying on libraries gives you the ability to make those choices yourself rather than being forced into only saying, "Well, I can only work with these lowest set of tools and I'm going to have to build everything on top of that as myself."

Orta Therox:        In theory, even the standard library is a dependency of yours. One of the weird things about actually using React Native as a native developer is I can fork and make changes to my foundational tools in a way that I've never been able to before. And it's hard to consider the actual baseline standard library as a dependency, especially if it's a close source one that's shipped to you and you have to deal with it. But the idea that that could be mutable and that you can make your own changes inside that and improve it is so amazing to me.

Alex Pounds:        So you're talking about React Native. How are you managing the dependencies within that and integrating that into their kind of X code build set up?

Orta Therox:        Basically React Native is a set of native code and a single Java script file. You have a translation process. It's very similar to the web where it kind of minimize it down into a single file that is then shipped inside your app. So, we consider our entire React Native project to be just a single CocoaPods dependency. So it just brings in all of its native dependencies and it brings in no Java script dependencies.

Orta Therox:        So, in terms of the app itself, it only runs inside CocoaPods for dependency management. There's no MPM, no yon, none of those. Those actually happened in a completely separate library and Repo. But from the perspective of the apps themselves, React Native is just some native code and a single JavaScript file and it just kind of does its magic together.

Alex Pounds:        So, you're then using Yarn or MBM somewhere else to generate that single JavaScript file?

Orta Therox:        Yep, that's exactly it. Part of our deploy process for React Native code is that it generates a single file and that gets put inside a get hub release that is then downloaded as part of a CocoaPods.

Alex Pounds:        How do you feel about this kind of melding of web technologies with native code?

Orta Therox:        Personally, it is exactly what I was looking for. We as a company, Artsy had really started to struggle, and native tooling and our ability to not controller and freedom to be able to deal with Apple's changes every year. Our apps we're getting bigger and they were taking significantly longer to make. Reusing web tooling meant that not only could we work significantly faster, but we could actually share code and concepts with our web engineers.

Orta Therox:        So, React Native, I don't how it's turned into React Native talk, but React Native ended up merging our web and IOS teams together. And it's been really good to go from someone that works on tooling for native to someone that works on tooling for Javascript because it actually is much more open. The tooling means making smaller pieces that can work together really nicely instead of trying to build monolithic tools like CocoaPods, and it makes it much easier to just dive in on a small project and contribute to other people's a workforce.

Alex Pounds:        And was that transition entirely smooth?

Orta Therox:        Yes, I think it was a smooth process. We as a team did two parallel tracks where one of us did Swift and CocoaPods and another engineer did React Native, and we tried to build a big project separately and atomically and trying to understand how that worked. And the React Native one turned out to be much easier to work with, much more maintainable and was much simpler code.

Orta Therox:        And so, we slowly, and this is the key, we took a very long time to make these changes, to turn workflow to be React Native without having a drastic rewrite. Yes, it's taken time and I'm still releasing tools on a weekly basis that make our lives easier inside the JavaScript world as native developers. But that's something that I could actually do.

Orta Therox:        Whereas in the native side, that ability was taken away from me because X code use to allow some sorts of plugins. But now it's very limited and it's very strict and wherever you can improve your own toolings.

Alex Pounds:        So how have you found managing dependencies in the JavaScript world?

Orta Therox:        That's a loaded question, but we actually came in at a really good time, in my opinion. We joined the JavaScript community about a year ago and Yarn must be somewhere around that kind of timeframe. We'd been struggling a lot with NPM being inside a dependency tree that is non-deterministic, and you have like no lock file and no ability to know how many versions of the same libraries inside your dependency tree has been very difficult.

Orta Therox:        Yarn fixed a good chunk of those problems for us. And then I believe MPM five was fixed about chunk of those problems, but the scale is still the biggest equites problem for us. I understand that Javascript doesn't have a standard library that's very comprehensive. And so, in order to have a useful standard library, you effectively have to build it yourself from this massive hierarchy of dependencies.

Orta Therox:        But just including React Native into a single project is 650 admiral dependencies. And previous to this, if we introduced the single dependency in our native code basis, you had to have read the code for almost all of it. You had to be able to say like, "Which other dependencies have you looked at?" And say why you chose this one.

Orta Therox:        Because it's a trade off in terms of understanding someone else's abstraction in exchange for whatever features you get for that dependency, and we lost the ability in moving to Javascript. We've tried a few times to try and reduce our dependency tree by finding the libraries at the admiral dependencies and try and reduce the dependency count, but it's really hard.

Orta Therox:        I've been building a tool called Danger in JavaScript and I have struggled so much to not just include a bazillion dependencies in order to get one thing done. You feel like you have to reinvent the wheel because somebody has already invented a nice wheel. Whereas the level that you should use a dependency is very different in JavaScript than it isn't native.

Alex Pounds:        I am really glad you mentioned Danger because I'm very intrigued by this project. Tell us what is Danger?

Orta Therox:        Danger is the idea of automating parts of the code review process. So, what we were finding was that in our native project, we were always maintaining a change log because we do big releases pretty often because deployments take about a week. So, so we wanted to make sure that on every single poll request, somebody had to include a change log entry.

Orta Therox:        The only way in which you could do that was by being able to tell every developer in the team, "We're going to all do this, and we're all going to remind each other on poll request if you don't remember." Doing that got boring very quickly, and we all kept forgetting and so I started building a generic system actually based off the color of CocoaPods to do that exact problem, to allow you to create your own rules that say like only on a Monday do you need to add a change log entry.

Orta Therox:        That's something you could totally feasibly built of Danger. We have things that warn you when you add a new dependency. Inside a JavaScript project, we have things that tell you that in order to make this Beta in a month time, you will need to ensure that these two numbers are the exact same inside this project, and it's really nice being able to automate massive chunks of your code review process.

Orta Therox:        And the way that I try to explain that is it's like trying to set baselines for the culture that you are accepting within your team. So we use it everywhere. Like if you make a contribution to CocoaPods then they'll ask you for a change log or check your tests and do all your Lintens. It becomes part of the CI process, and a way of communicating back onto the code review poll request page.

Alex Pounds:        Where does the name come from?

Orta Therox:        The name comes from my wife who is looking at me suspiciously. We tried to give it a name that was not Danger, originally going for Columbo, the detective in the '60s. But we ended up finding that all of the Ruby Gems were not available. And Danger is my wife's nickname for six years, seven. And so, it felt like a really good mix of like, what I was actually going for, and a fun in joke, but then it turns out to Danger was actually a multi-year process.

Orta Therox:        I think I started work on it around the time that Swift package manager came out because wasn't necessarily worth my time to work on CocoaPods too much anymore. So I started working on that. And Danger today is something that runs on UCI, but internally Artsy, we've been using it at a larger scale.

Orta Therox:        So I built a thing called peril. Imagine every single poll request on an entire org could get some arbitrary rules. So, an example I'm working on at the moment is a spell checker on any markdown file that gets poll requested. So, it gives you the ability to set all level of cultural rules instead of repo level of cultural rules.

Orta Therox:        I'm really interested in how we can improve process and obviously like get hub a building for the vast majority of people and to expect them to be able to include every single teams, individual features, and requests is quite unreasonable. So, using Danger and Peril, you can start creating your own workflows inside Gethub or Bitbucket or Get lab.

Alex Pounds:        Why did you choose to build on top of the CocoaPods call?

Orta Therox:        The good bits about CocoaPods is actually the bit that we talked about earlier. The touring completeness part of it. Danger uses the same sort of evaluate and turn it into an object system that allows you to create your own rules. So, it allows you to arbitrarily say like, "This is a fail. This is a warning. This is a message," based on if statements throughout the Danger file.

Orta Therox:        CocoaPods already had a lot of that structure and so, it was much easier to take an existing code base that I knew and start working from that upwards in terms of the original version of Danger for Ruby. Whereas when I rewrote it for Javascript, which is a great use of my time, I had just built it from scratch with all these ideas of how Danger has been built and what a revised version of the idea of Danger could be said.

Alex Pounds:        Do you plan on maintaining the Ruby and JavaScript versions long-term or choosing one and kind of focusing on that?

Orta Therox:        I planned on keeping both around. Danger and Ruby is the one that is effectively done. It's nice because having worked on something like CocoaPods for so many years where it cannot be done, it's actually nice to be able to say that like, "This project works, has hundreds of thousands of downloads on Ruby Gems, and is stable and people can rely on it. And maybe I get one issue a week, and so I'm really happy with the position it's in."

Orta Therox:        And so, a lot of my focus has been on Danger JavaScript because in the rewrite, I could apply constraints I didn't apply on the original Ruby version, which makes it easy to evaluate on a server instead of on the CI level. Which means it's extensible in ways that the original is not. And that's what gives the ability to build something like Peril. For me, Danger JavaScript is a more intense version of the idea that allows you to do a lot more.

Orta Therox:        But at the trade-off, it's in the JavaScript world, so it's definitely not stable. I mean that stability in the sense of all of the dependencies are changing, the Danger has dependencies that you probably have your app as well. So, when they change, you don't necessarily have the same guarantees of it continually working so well.

Orta Therox:        So, I'm still figuring out the space where stability is for Danger JavaScript, but it's getting there. At one point, I audit maybe two or three weeks ago because I felt like it was at point now where anybody can use it for most projects. But even today, I was debating something that I think I would call a 2.0 release because it will be a breaking change.

Alex Pounds:        What would the breaking change mean?

Orta Therox:        Some of the problems of running something like Danger is that I want to be able to safely evaluate code because if I want to put this on the server, I'm literally arbitrarily evaluating code, which as Andrew mentioned earlier, is a bad idea for anyone that wants to be hosting something.

Orta Therox:        And so what I did was actually used just the test runner in the node world to like actually provide a mocking system and white listing and black listing parts of note that I would like to expose to people so that I can safely evaluate code. But I came to the conclusion that maybe I could do it with less dependencies if I just do some of my own work instead.

Orta Therox:        So I would like to test that out. And if that works, then removal of the just dependency is probably what I would consider a breaking change because it would have further changes to people's projects I think.

Alex Pounds:        So, you want to end up with something like running JavaScript in the browser with a limited set of capabilities and no access to the system or to be able to change the runtime?

Orta Therox:        Yep, exactly. So I'd like to be able to run a Danger server that anybody can just click a button and be able to say, "Please run Danger on this repo. And every time there's a poll request on it, then generate these rules." But in order to do that, I need to evaluate arbitrary JavaScript code that could be coming from anywhere, and the safety of my server is pretty important in those cases.

Alex Pounds:        Especially when you have other peoples get hub keys on that server.

Orta Therox:        Yes, that is definitely true. I really want to be super cautious about the rollout of that itself. A lot of the things that I have been thinking about is like how do you safely evaluate JavaScript? Which is tough, but luckily a lot of people are putting a lot of time into that. Whereas in the ruby world, there's less people that are involved in, "How do I safely evaluate Ruby?" Because, it's not really a problem people have

Alex Pounds:        Ruby does have like a safe mode that you can basically stop it from being able to talk to the system that's it's running in. Alternatively, a lot of people are doing things where they're running code from a third party or inside of a container, basically hiding those things away, which doesn't necessarily keep it completely secure.

Alex Pounds:        I think you have to actually put it inside a VM. Put a container inside of a VM to make sure that you can actually escape properly, but that is quite a lot more infrastructure than just exacting a JavaScript file.

Orta Therox:        I'd really like to see some more advances in that and make that even easier. The other day I found myself thrown back a decade in that I had to download a palm file from a cam.ac.uk website and install a whole bunch of modules via see pan because I was trying to extract some data from a palm pilot calendar file. And as I sat there watching my terminal, watching hundreds of dependencies get downloaded, compiled and run from the software.

Orta Therox:        It made me think I really wish it would be super easy to just say, "Run this inside a container," because although I kind of trust it, I would like to entirely trust it and have it sandbox off somewhere else.

Alex Pounds:        Yeah. I think sandbox and independency manager is definitely something that more dependency managers should look into. Like the Mac itself and most OSes come with pretty good sandboxing tools, you just have to know all the C libraries and all of the hooks that you need to do, but we managed to get CocoaPods doing it with maybe a week worth's of work. So, I think it'll be a good move for a lot of them.

Andrew Nesbitt:     Especially when you come to things like posting schools, scripts for node modules that have full access to your local shell.

Orta Therox:        Yeah. There's been reports of people having fake node modules that have the exact sounding names that just happened to take your tokens and ship them up to some centralized server somewhere.

Alex Pounds:        Have you had any similar problems whilst working on CocoaPods?

Orta Therox:        No. CocoaPods could in theory have a post install hook be a script. So, you'd have to have a mid-file or a Ruby file or whatever. But no one to my knowledge has ever submitted a, "This is a malicious pod SPEC." So, I assume not the closest community has ever come to having like real CVEs, like real networking security problems is when one of our biggest dependencies called F networking shipped something that had a bad SSL pinning, and that's rarely been it for the entire Native and IOS community.

Orta Therox:        I think generally, most people rely on Apple's tooling to deliver a lot of those securities issues. And so, Apple are the ones that are updating regularly and most of us are just using their existing APIs to build upon it. The SSL pinning, for example, was custom code that was added to a networking library. And now that sort of code is now something that Apple provides. So, they don't have that feature anymore.

Orta Therox:        So, that security problem doesn't feasibly exist unless you recreate it. And most people are not willing to recreate Apple's projects because Apple's projects tend to be very polished the way it should be, and usually, pretty reasonably maintained.

Alex Pounds:        So, normally at this point we would ask you if somebody wanted to get involved in CocoaPods, where should they go to learn that? And while that question stands, this one also comes with a question, should people be getting involved with CocoaPods at this point in its life cycle?

Orta Therox:        I think a bit of yes and no. We've definitely have the hundreds of issues now on the CocoaPods repo, and there's definitely a lot of space for people that want to be involved in the project because it's a product used by millions of projects. Realistically, now is actually a really good time if you want to contribute to something that is at the scale.

Orta Therox:        A lot of people they want to work on the open source because it gives them an impact they do not have in maybe in their workspace. Like maybe, you're working on a startup where you can only ship an app to a few thousand people, but you could work on CocoaPods and get a feature used by all of your peers. So there's definitely space to do it.

Orta Therox:        But realistically, I would just be recommending people like actively work on bug fixes that are annoying them. Because, CocoaPods definitely doesn't have the resources anymore to actively go out and spend time just fixing other people's books. The only things that are really getting fixed now are the things that like actively detrimental to everybody or are actively being bugs that the maintainers see.

Orta Therox:        So, it's a good space to like start picking up some of the easy issues because we do try and label them.

Alex Pounds:        And if people wanted to learn more about CocoaPods, where should they go to do that?

Orta Therox:        Cocoaods.org, or follow CocoaPods on twitter.

Alex Pounds:        And what about you? If people wanted to learn more about you, where should they go?

Orta Therox:        So I am at orta.io, because everyone's on that io domain nowadays. And I am also at twitter as well. Having a first name that is four characters that's relatively unique globally is super useful because it means I get it on almost every single service. So whatever it is, I'll probably be alter on it.

Andrew Nesbitt:     Well, this has been really interesting Orta, fascinating to kind of hear about the plans to kind of deprecation of CocoaPods and how you are thinking about it continuing to exist despite Apple trying to squish it, and to hear about Danger, which is a really exciting project that you've kind of, I guess naturally moving towards now that CocoaPods is winding down. Thanks so much for coming on and we'll catch up with you again soon maybe.

Orta Therox:        Impressive. It's been a pleasure Alex and Andrew. Have a good one.

Andrew Nesbitt:     Thank you. Have a great day.

Orta Therox:        Cheers.
