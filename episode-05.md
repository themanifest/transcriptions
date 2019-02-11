Episode 5 - Pub with Natalie Weizenbaum  - https://manifest.fm/5
--------------------------------------------------------------

Alex Pounds:        Welcome to The Manifest, a podcast all about package management. My name is Alex Pounds.

Andrew Nesbitt:     And I'm Andrew Nesbitt.

Alex Pounds:        And together we're exploring the technical details of package management stories and the history of various projects and the communities around them too. Today we're joined by Natalie Weizenbaum, maintainer of the pub, she's the package manager for Dart. Natalie, welcome to The Manifest.

Natalie Weizenbaum: Thank you.

Alex Pounds:        So before we get started, do you want to give us an idea of your background? How you got into computer science and ended up as a maintainer of a package manager?

Natalie Weizenbaum: Well, I kind of got into computer science in the most straightforward way imaginable. I was in college and I had a free course, one of my first few quarters and a friend of mine was taking a computer science course. So I decided to join him and I found that I loved it a lot. That was how I got into programming in general.

Natalie Weizenbaum: From there, I kind of hung around on some mailing lists, got started doing open source work, got internships and ended up working for Google. I'm working on the Dart project on the package manager as well as the test runner and various other packages in the Dart ecosystem. And that came about mostly because I've been interested in programming languages my whole career, I also work on the SaaS, CSS compiler in my 20% time at Google, which is also a programming language focused project and sort of inspired my interest in that area.

Alex Pounds:        Do you want to explain a little bit about Dart for people who may have heard the name of the language but maybe don't understand exactly what it is and where the design decisions were made for it?

Natalie Weizenbaum: Sure. Dart is a language that's focused on targeting a number of different platforms with the same underlying core libraries and language so that it's easy to move code from platform to platform and move expertise from platform to platform. The two main platforms that we are focusing on right now are the web has a mode that compiles to JavaScript and runs on any browser and there are frameworks out there that you start. There's a AngularDart which is widely used at Google and people are starting to pick it up externally as well.

Natalie Weizenbaum: There's also Flutter which is a mobile framework for Dart that uses some of the same design principles as Angular, but in a more mobile focused way.

Andrew Nesbitt:     Is that for deploying to Android apps or for mobile focused websites?

Natalie Weizenbaum: Android and iOS apps. When using Flutter Dart compiles to native code, that can be run directly on the operating system of the phones in question, which makes it very efficient. It's a statically typed language which gives you more checking than you'd have if you were using plain JavaScript on the web and also makes it easier to compile for mobile app usage.

Natalie Weizenbaum: It's also a modern language with a lot of the features that you'd come to expect from languages made in the last five or 10 years, type in France and clean object systems, that sort of thing.

Andrew Nesbitt:     Dart is a language which came from within Google, much like go correct?

Natalie Weizenbaum: That's right.

Andrew Nesbitt:     What kind of use cases did Google have in mind for it when they created it?

Natalie Weizenbaum: Originally the idea was to create a language that was more well-suited for large web applications than JavaScript, so Google has mountains of JavaScript code and we've done our best to try to make that code as maintainable as possible, but doing that relied heavily on bolting on a type system, on top of the existing JavaScript semantics. Recently, projects like TypeScript have come along to sort of push that idea forward, but there's still a lot of corners of JavaScript as a language that make it really difficult to write large applications that many people work on and have them be well-factored and efficient. And Dart was designed to solve that problem.

Alex Pounds:        So that was created within Google and was one of the things that Google has open source. What has the adoption outside of Google and being like formal language?

Natalie Weizenbaum: We've been mostly focusing on internal adoption for the past few years, so it is not been picked up as much externally as maybe we would like. I think right over the next couple of years we're going to be making much more of a push for external adoption. Now, that we've taken feedback from our internal users and folded it back into the language, now that Flutter is starting to become more mature and more usable for real world applications. There's a very good opportunity now to start pushing for more external adoption and that's one of the things that I'm most interested in, in my role in the Dart project. I'm mostly focused on working with the external package ecosystem and Pub which is effectively only used in the external world. So I'm personally very invested in making it a great language for people outside Google to use.

Andrew Nesbitt:     I was going to say that it's quite interesting to look at how the package management in Go is so different from the package management situation in Dart, given that they both come from a corporation that traditionally does package management internally in a very different way to how most modern languages handle those things.

Natalie Weizenbaum: Yeah, I mean Google's a big place so the teams that worked on Dart and Go are, as far as I know, totally separate. I don't know of any people who have worked on both. When Dart was pre 1.0, when there was a lot of churn going on in the language. One of my coworkers, Bob Nystrom, recognized that in order to be usable in the real world, there needed to be away to distribute and reuse libraries. You see this in every language, but he came at it from the perspective of someone who had done a fair amount of work outside Google, outside of the monolithic repos structure, and as someone who was familiar with the packaging systems of languages like Ruby and Python and JavaScript that were very popular in the outside world.

Natalie Weizenbaum: So he was the person who originally started working on Pub and started pushing the team to look at how Dart as a language would be used in the external world as opposed to within Google. And soon after he started working on that, I became interested because I've also spent a lot of time working in the external world, especially in the Ruby community. And I also had a fair amount of background working with package managers and maintaining small packages of the sort that you see in those ecosystems.

Natalie Weizenbaum: So I wanted to help make Dart a good platform for building an ecosystem and not just a chunk of things that Google released into the world. So together we worked on building a package manager that we thought would be effective at fostering that and I think we've had a fair amount of success. Dart's ecosystem isn't huge yet, but we definitely have a lot of packages contributed by a lot of people that tend to be pretty high quality and I'm proud of that.

Andrew Nesbitt:     So tell us about Pub. How does Pub do package management?

Natalie Weizenbaum: So at a fundamental level it uses the specification and lock file model that I believe Bundler first pioneered. So you have what we call a Pubspec which is a Yammel file that declares all packages you depend on and what ranges of versions are acceptable for each of those packages.

Natalie Weizenbaum: You tell Pub to install the packages in a Pubspec, it looks at all of those dependencies, downloads, packages, looks at their dependencies and tries to come up with a set of packages that satisfy all of the dependencies transitively throughout the universe of packages that your package requires.

Natalie Weizenbaum: Then it writes its solution to a lock file, which it uses for future version solves to try to make sure that there's as little churn as possible, and that's the fundamental principle, which is pretty similar to how a lot of package managers work. I know most of the JavaScript package managers don't, I think there are unusual in that and Yarn is starting to push more towards the Bundler like model in JavaScript, but and Cargo does the same thing probably some others.

Andrew Nesbitt:     It seems to be Pub is ticked all the boxes when it comes to covering all the features that it needs to be a reliable dependency resolver as well as package installer. Am I right in thinking that Pub can also be used to install Dart applications and kind of almost like a build tool in the same way that may even allows you to do both build the tool and managed the tools dependencies?

Natalie Weizenbaum: So there is a mode in Pub that builds your application. It's not as fleshed out as it could be, there's a lot of gnarly history behind it that is probably not worth getting into here, but the long and short of it is that we kind of shoehorned some build logic into there because there wasn't really a better place for it in the ecosystem. And we're in the process of trying to create that better place now.

Natalie Weizenbaum: So while Pub can do some degree of building a project, we think it's better to have a more dedicated tool that knows how to build a wider variety of things, then Pub could really encompassed in itself and we're working on building that now.

Andrew Nesbitt:     So one thing you touched upon earlier was that Pub is mostly used by external dot uses. How does Google do package management internally?

Natalie Weizenbaum: So the way Google's internal repository works and this is beyond Dart, this is for everything, is that there is one single version of any piece of software across all Google. That means that you don't need the version resolution logic that Pub has. You just declare, "Oh, I use the string scanner package and there's only one version of it, so you pull it in and it's great," and there is a whole set of protocols internally for what to do if you are pulling in and upgrade of a package that has breaking changes but because it's a single repository, you can just create one commit that brings in a new version of a package and upgrades all of the uses of that package as necessary.

Andrew Nesbitt:     So imagine you have some fairly extensive test suites, that essentially if you update one Dart package internally, it would kick off tests for all the Dart code that would depend on that package.

Natalie Weizenbaum: Yeah, there's very sophisticated build tools that are clever enough to only run the tests so that it can tell are effected by the change and make sure that they don't flake and so on and so forth. It's very impressive, I try to not deal with it as much as possible because I'm more interested in the external world, but every now and then I'll have to bring in the new package. It's a lot smoother than it could be given how much code there is and how painful breaking changes can be.

Andrew Nesbitt:     And that definitely showed in the initial versions of Go with Git essentially it looked like that was designed to work really well with within Google, and had very little consideration for the users who existed on the internet and would say, go get me this arbitrary Europe and I hope it's still the same as it was when I downloaded it yesterday.

Natalie Weizenbaum: Yeah, I think Dart has been fortunate to have people working on it who are experienced with the external world and with how other programming languages work outside of Google. Certainly Go has taken off, but I think that was much more driven by external users writing tools to make it work in the external way, whereas Dart has had that more out of the box upon release.

Andrew Nesbitt:     So I was looking around the source code of Pub as I do just randomly poking my head inside of package managers and I noticed that Pub has its own Pubspec. Does that mean that Pub uses Pub to install its own dependencies?

Natalie Weizenbaum: That's correct. It wasn't always this way, it used to live inside the Dart SDK repository, which has its own system for installing stuff that is based on G client, which is a suite of software I don't understand very well. But I refactored it pulled Pub into its own repository because it's nice to have things well-factored and it's nice to be able to work on it without having to bring in updates from, you know, the Dart VM being changed or whatever.

Natalie Weizenbaum: So the way that works is every Dart SDK has a pre-compiled version of Pub that just bakes in all of its dependencies and pops out Snapshot which is basically Byte Code of a Dart application that has all of its dependencies baked into the binary blob. So we can use that Snapshot to get the dependencies while we're developing Pub and we tend to use an older snapshot. But as long as we don't use the most bleeding edge features of Pub in Pubs on Pubspec we are able to bootstrap it and work on it as though it's just a normal Dart package.

Andrew Nesbitt:     And that's I guess one of the benefits of having a language that you compile rather than a scripting language?

Natalie Weizenbaum: Yeah, definitely. And Dart does have a scripting mode, you can certainly just run it straight from source, but it also has this compiled mode which lets us do this.

Andrew Nesbitt:     That sounds really handy. And probably for deployment aspects as well and not requiring Pub to install and bootstrap itself before actually needing to pick up the application dependencies.

Natalie Weizenbaum: Yeah, that's actually one of the things that's been useful in my alternate life as a maintainer of Sass. I've been working on porting Sass to Dart and one of the reasons that I chose Dart it's not just because I work on Dart, but it's because Dart has really great support for creating totally standalone applications that don't require a lot of bootstrapping that don't require a full dark install on the target machine. You can just have the Dart VM executable and the Snapshot and that's all you need to get up and running.

Alex Pounds:        Before we delve into the details of your alternate life as a Sass maintainer, one of the things you mentioned in passing is about using some of the bleeding edge features in Pub and how you avoid that to make sure that it can bootstrap itself, but what kind of features are there in Pub that are newish or more advanced than you might see in other places?

Natalie Weizenbaum: So the biggest new feature is actually called features, which is something that we stole, borrowed from the cargo package manager for rest. The idea is that a package can declare that it supports a feature which doesn't necessarily have to be enabled for the package to work properly, and each feature can have its own dependencies. So this lets you express the idea of optional dependencies for a package. So you can say if this package is being used on Flutter, for example, for mobile applications, then it depends on the Flutter drawing library. But if it's being used for the web, all it needs is canvas, so it doesn't need to have that dependency, which gives package maintainers a lot more flexibility in adding dependencies for things that may not necessarily be useful for all of their downstream consumers without having to worry about increasing the installation, wait for everyone, or using packages that just aren't available on the platforms that their consumers are using.

Andrew Nesbitt:     Is that something you describe in the Pubspec or is it worked out from analyzing the way that the code is that to use?

Natalie Weizenbaum: No, it's purely in the Pubspec. Pub has a philosophy of only being interested in the shape of code as it relates to dependencies. So Pub cares almost not at all about what the contents of the package is outside of the dependencies, it will do some light validation before you publish to make sure you're not doing anything silly like importing a package you don't depend on, but as far as dependency resolution goes, it's entirely based on what's in the Pubspec and that helps keep it clean and comprehensible for users who might be new to Dart and don't have a thorough understanding yet of how the package manager works and are still sort of feeling their way through it. Having a simpler model is very useful for that use case.

Alex Pounds:        You might've just answered my next question in that previous answer, but I was curious about whether the cross platform nature or the multi-deploy target nature of Dart that has any implications for Pub and how Pub structures is work?

Natalie Weizenbaum: To some degree, yes. Certainly there are packages that are only suitable for certain platforms, we have some features in the Pubspec that allow you to declare, for example, I depend on this version of the Flutter SDK if you use Flutter specific features. And the features feature is heavily motivated by the fact that there are packages that target different platforms and then there are also packages that want to be cross platform and may need to use platform specific packages depending on which platform they're targeting.

Andrew Nesbitt:     Does the Pub website that you actually search or look up packages by feature?

Natalie Weizenbaum: I believe so. That's actually not part of what I maintained, so I'm not 100 percent up to date on what it does, but I have seen flags on there for this is a Flutter package, this is a web package, this is a server only package, that sort of thing.

Andrew Nesbitt:     I seem to remember the PHP package manager has a similar kind of idea where I don't think it actually breaks it down on a per feature or multiple features in a given package, but you can say there are package essentially provides this functionality and there's a virtual namespace or a name of a package that can be implemented by a number of different packages and essentially the package manager will decide, "Oh, I've already pulled something in to provide those features. I don't need to go and get another third party library to be able to fulfill your request for say, a HTP client because we already have one that we've partly installed somewhere else."

Natalie Weizenbaum: Yeah. We talked about an approach like that for supporting cross-platform packages, but my opinion is that it makes the ontology of which packages exist in the world more complicated when you can have something like a virtual package which is implemented by another package that makes there be a lot of different types of thing, which I think makes it harder to comprehend exactly what's going on.

Natalie Weizenbaum: Then if you can model it with, there's just a package that is an http client and that package might know how to give itself a mobile client or a web client, but it's still just a package which might have different dependencies depending on which options you select.

Andrew Nesbitt:     Yeah, I can imagine the transitive dependency resolution getting really hairy as you start getting circular dependencies on virtual dependencies.

Natalie Weizenbaum: Yeah.

Andrew Nesbitt:     Does Pub have any features or ability to reach out to system level dependencies, so binding to see libraries or other third party code that's not managed directly by Pub?

Natalie Weizenbaum: Not currently. We've talked about that along at a couple different axes because Dart is mostly focused on the web and mobile, there hasn't been strong demand yet for see interop, there's some support for it in the Dart VM but there just hasn't been enough users clamoring for it to be worth allocating the time to make it work as part of the package manager. There's also been some talk of making Pub work with other package managers, especially JavaScript package managers to install JavaScripts code alongside it, but again, that just hasn't been something that enough users have asked for to make it a priority in terms of implementation.

Andrew Nesbitt:     So would that be things like saying you can install this package which will work with your application, it may not be written in Dart, it may be written in JavaScript and it's available over on the MPM registry rather than the Pub registry?

Natalie Weizenbaum: Something like that, yeah.

Alex Pounds:        Is that something that's generally supported? Can Dart use JavaScript dependencies?

Natalie Weizenbaum: Dart can use JavaScript code. Pub itself doesn't have support for using JavaScript dependencies, so you'd need to either create a new Pub package that contains JavaScript code, which people have done for a couple of things I think, or you'd need to have some separate out of band process for running NPM or something like that to download the JavaScript and put it somewhere that your Dart code can find it.

Alex Pounds:        So this might be a good jumping off point into the other aspects of your open source work, where you are maintaining Sass and you told us a little bit about that before, but for people who might not have touched it, what does Sass do?

Natalie Weizenbaum: Sass is a CSS pre-compiler, it's actually the first CSS pre-compiler. So it takes style sheets written in the language Sass or SCSS which is the CSS compatible syntax for Sass, and adds a bunch of features like being able to do arithmetic, having variables, having mixings, being able to extend selectors, and runs all of those features and creates a plain CSS file which can be served directly to a browser.

Alex Pounds:        And you described it as a pre-compiler and that's very much how I think of it as a kind of a compiler, but there might be some overlap there as well with the package management skills.

Natalie Weizenbaum: To some extent, there is a Sass package manager called Eyeglass, but I actually don't work on it, that is the brainchild of Chris Epstein who is my partner on Sass, and I'm not super familiar with the ins and outs of how that works. I mostly focus on the language and the ethics of Sass.

Andrew Nesbitt:     That is one that I have not looked at all, I remember it popping up back when I used to maintain a library called note Sass which was a JavaScript bindings to LibSass, which is the C++ version of Sass as there's quite a few different, Sass' bouncing around. And I remember it popping up when I was maintaining it, but I don't think I've ever actually used it. I have to look into that one, that sounds quite interesting. As a Sass is cheering complete, am I right thinking?

Natalie Weizenbaum: That's right. It has many of the features you'd expect of any other programming language, it has lists, it has functions, it has loops, it is really a programming language in and of itself, it's just one that's very informed by the fundamental goal of writing CSS.

Andrew Nesbitt:     And potentially some interesting ways of looking at CSS especially around the module side of things where Sass actually kind of made it conceivable to write and reuse pieces of CSS or Sass without pulling your hair out when you actually to try to deploy all of that into one style sheet.

Natalie Weizenbaum: Yeah, that's actually an intersection between Sass and package management. One of our fundamental goals with Sass has always been to make it possible to reuse styles in a modular way, and it's taken a long time but that's finally starting to happen between eyeglass and style systems like Bootstrap and Susie. Very exciting to see Sass enabling people to distribute and re-use chunks of style between websites.

Andrew Nesbitt:     Does Dart do any of the CSS in JavaScript things or is that basically just going to be laid on top of whatever you're writing in Dart?

Natalie Weizenbaum: I'm not super familiar with the details of Dart's web frameworks, I mostly work at a lower levels on that on the infrastructure that they use. So it's possible that AngularDart does some stuff like that, but I'm not aware of it.

Alex Pounds:        So let's switch back to talking about Pub. I understand one of the things you're most excited about is Pub's version resolution algorithm and the work that you've been doing around that.

Natalie Weizenbaum: Yeah, so right now today, Pub's version resolution algorithm is okay, version resolution is a really, really tricky problem, it's NP complete, which means that it's really easy to tell if a given set of packages is correctly resolved, but it's really, really hard to find a set of packages that matches a set of dependencies. So every existing package manager does the best it can to find a resolution for the dependencies it's given.

Natalie Weizenbaum: And if any package manager, we're able to do it very fast all the time that would be a major computer science breakthrough, so no one is hoping to do that, but many package managers do pretty well at going pretty fast for most realistic inputs. Right now Pub does that too, but there are some inputs that it's still very slow for even inputs that are kind of reasonable and the errors it produces are not great and I think that's a problem that's shared among many package managers is that when they fail to find a set of dependencies that are valid for the constraints they're given, it's hard to produce a good error there because the reason that the dependencies aren't valid might be very complex.

Natalie Weizenbaum: It might involve, "Oh well this one package has a slightly too narrow version range," which means we have to select a slightly older version of this other package which has version range that's too high to select a version of this third package which you depended on a lower version of in your Pubspec or whatever. It gets really complicated.

Natalie Weizenbaum: So what I've been looking at is ways to improve those algorithms and to have them produce error output that sort of tells that story that says, "You need to look at this package because you depended on it from this package because that package isn't compatible with current SDK. And we can't choose the latest version of that package because of this other thing." So I've been thinking about this for more than a year at this point, I've been banging my head against it in a bunch of different ways. But I think I have a path to an algorithm that will make it work well and I'm hoping to do some serious work on this in the next few months.

Natalie Weizenbaum: So when you're talking about NP complete problems, this is an area that's been studied very extensively in the computer science literature and there's this technique called Answer Set Programming, which is basically domain specific programming languages for expressing search problems that our NP complete.

Natalie Weizenbaum: For example, the problem of searching for a set of packages that satisfies a whole bunch of transitive dependencies and there are systems that do this really effectively, and that use an algorithm called DPLL which is named after four guys who invented it in the '60s who's name I can't remember, that these algorithms are all based on. And the idea behind DPLL is that you look at the constraints as a bunch of logical sentences, and you combine these sentences in certain ways and you guess ... Well, what happens if I set this variable to this value? Which sentences does that satisfy? What other variables does that force?

Natalie Weizenbaum: And you do this iteratively until one of your guesses fails, I can tell that all of these variables set in this particular way or not a valid answer. So in the case of package management, that would mean, I can tell that selecting the latest version of every package is not a valid answer because some dependency isn't satisfied there. And the way DPLL handles this is not just going back and trying a different solution, which is kind of what most package managers do today.

Natalie Weizenbaum: It actually learns from that error and it adds a new logical formula that expresses why that error happened, and includes that logical formula in its reasoning from then on. So it avoids going down rabbit holes that it's already explored. So for example, in a package management context, that might mean it avoids trying to select the latest version of package foo when every other time it hasn't worked because of package bar. So it knows that every time you have package bar selected, it's not even worth looking at package foo, so that helps improve the speed.

Natalie Weizenbaum: But also what you get is these logical sentences as sort of a byproduct of this. And what I'm hoping to do is reinterpret the logical sentences back into the realm of package management, and use them to tell users well, "Okay, I have this logical sentence that says foo and bar are incompatible." I can tell the user this. I know how that sentence was derived so I can say because foo has a dependency that's incompatible with this other dependency of Bar, we know foo and bar incompatible. And that's why we couldn't find any packages that match your Pubspec.

Alex Pounds:        I can tell you that the DPLL algorithm is the Davis put them, log them in Loveland algorithm and it is an extension of the Davis put them algorithm, which was an early resolution based procedure. You might be able to tell that I have Wikipedia open in front of me, but your algorithm is very much going to be, it's not merely an implementation of this, it's actually a going a step further, right?

Natalie Weizenbaum: To some degree, yeah, and I think Answer Set Programming has taken this step but not in a way that's specific to package management. So my goal is to look at the extensions of DPLL that Answer Set Programming uses and sort of pare them down until they're the minimal sort of thing that is useful for package resolution in particular and then try to add graceful error handling on top of that.

Alex Pounds:        So your hypothesis that you mentioned is that this is going to be a lot better, it's going to be a real step forward for package management and for Pub. Have you had any thoughts about how you're going to test that hypothesis? How are you gonna test your new algorithm and these results and make sure that it does have those results that you expect?

Natalie Weizenbaum: Well, Pub already has a fairly wide test suite of tricky package resolution situations, we also have a number of user reports of situations where it takes longer than it should right now. There's also a cross platform version solving algorithm written in Ruby called Molinillo that has its own test suite that I'm hoping to use to verify that this new algorithm works well. And to some extent, it's hard to test that it's better because honestly most of the algorithms right now are pretty good in terms of solving things efficiently and they may even be tweaked to be good for all of the known test cases.

Natalie Weizenbaum: So to some extent, I think the best validation will be seeing how many reports we get of package resolution taking too long once we put this algorithm and to use in practice.

Alex Pounds:        One of the things that we haven't really explored yet on The Manifest are those tricky situations that you mentioned, the common corner cases and problematic cases for resolving packages. What kind of issues are the common ones there?

Natalie Weizenbaum: It depends on the specifics of the algorithm you're using. So things we've seen in Pub involve packages that are changing their major version very often because that invalidates a lot of the assumptions of semantic versioning, you end up having packages that have dependencies that can't be met because some other package depends on the latest version of something, and this package hasn't been updated recently enough to be compatible with the latest version. But I don't think any of those are inherently going to cause package resolution to take a long time in all circumstances.

Natalie Weizenbaum: My intuition and I haven't verified this, but I'm fairly confident that it's true, is that as long as there's a simple explanation in a human sense for why packages can't be resolved, it should be possible for an algorithm to solve them efficiently. I believe that the only cases that should get really, really gnarly are the ones where packages are just doing things that are weird and unusual. Like every version of a package has a completely different set of dependencies that have their own completely different sets of dependencies for each version, which inherently is going to be extremely difficult to resolve.

Natalie Weizenbaum: But I think as long as packages look fairly consistent in the way they tend to, it should be possible for an algorithm to resolve them effectively.

Andrew Nesbitt:     When pub is walking through this dependency resolution algorithm, is it actually requesting the information about each versions dependencies from the registry as it runs into them or is it collecting them all up front before it starts to work?

Natalie Weizenbaum: It requests them lazily, so as it encounters a package it requests from the server a list of all of the versions of that package and all of their dependencies. All of the versions of each package are fetched eagerly, but it doesn't fetch the entire world of all packages published on Pub, it only fetches a package as it encounters it in the version resolution process.

Andrew Nesbitt:     And that gets slower I guess as more packages have more versions that are published and the whole network gets deeper and wider for the size of the potential number of trans of dependencies, have you thought about having any kind of index of those dependencies that Pub could pull from to kind of speed that process up?

Natalie Weizenbaum: We've talked about it a little bit, I think the main problem would be in the depth in terms of a given package, having many, many, many versions. So far we don't have any packages that have enough word. That's a problem. I could imagine something though where the package server is just clever enough to filter out packages that aren't compatible with the SDK of the version of Pub that's doing the fetching. In which case, it would almost always only be on the order of maybe a hundred versions of a given package, which is a trivial amount of Json to send down.

Andrew Nesbitt:     Yeah, I think Bundler actually ended up forcing the RubyGems server to implement something like that, a compact index of all of the versions and the dependencies because it was actually causing quite a lot of database strain for pulling out all of those things before sending them down over the wire.

Natalie Weizenbaum: Yeah, Dart is nowhere near as popular as Ruby right now. So it's gonna be awhile before we need to worry about that a huge amount and we have some flexibility in terms of improving the protocol over time. We can always have older versions, use an older protocol and gradually move newer versions to a new protocol without risking too much breakage.

Andrew Nesbitt:     I guess you also have the ability to ramp up Google Cloud platform or put some of the Google as kind of sized infrastructure behind the registry that RubyGems doesn't necessarily have the resources to be able to do.

Natalie Weizenbaum: Yep. We have everything on Google app engine right now, which gives us a fair amount of flexibility in terms of ramping up.

Alex Pounds:        So Dart smaller ecosystem has allowed you to not worry about the efficiencies that some of the other package managers had to worry about, but it also means that you've got a fairly low bus factor as in it wouldn't take that many team members to get hit by a bus for Pub to really be in trouble. Have you had any thoughts or do you have any plans on ways to improve that and get more people involved in the project?

Natalie Weizenbaum: I'm not too worried about it right now. Right now, I think we have four people who are familiar enough with Pub to continue working on it. If for example, I got hit by a bus and I think the code base is straightforward enough and I've taken pains and so has Bob my co-worker, to document it very thoroughly that it would be possible for a skilled engineer to come in and ramp up on it pretty quickly without necessarily any guidance, although obviously that's not ideal.

Andrew Nesbitt:     I guess on the infrastructure side, Google app engine will take care of a lot of the kind of ops work. You don't necessarily need a full time ops person babysitting the servers.

Natalie Weizenbaum: At this point, I think that's definitely true. Certainly once the Dart ecosystem starts to blossom, there may be more need for more dedicated support there, but that's also something that we can bring in if necessary.

Alex Pounds:        And I guess that is also a nice benefit of being a Google supported projects. We don't want you to get hit by a bus, but if it did happen then they would have the cloud and the resources to ensure that Pub continues on.

Natalie Weizenbaum: For sure.

Andrew Nesbitt:     Do you imagine that Dart and Pub would, as they get used more outside of Google, potentially bring more community governance rather than being solely a Google project?

Natalie Weizenbaum: I would love to see that. Right now we don't have a lot of external contributors clamoring to submit stuff. We have had a few substantial contributions to Pub and actually one of the core packages in the ecosystem, the HTTP package is currently being refactored by a community member shout outs to Don Omstead. I think there's definitely room for more community governance once we have more community members contributing code and having the time to be actively involved in the project.

Natalie Weizenbaum: Right now it's not something we thought of a huge amount mostly because there's no one who's expressed a huge amount of interest, but all of these projects are on GitHub and I would certainly be excited to welcome anyone to contribute to Pub and provide their ideas, and if they build up enough momentum to become a committer I would have to check to see what Google's policy on that is though.

Alex Pounds:        I'd imagine most Google presidents have the contributed license agreement and a certain subset of open source licenses.

Natalie Weizenbaum: Yeah, we definitely have the license agreement, you have to assign copyright to Google for corporate reasons. I believe our license is just MIT or something like that, so it's certainly possible for a community member to fork Pub and do whatever they want with it. Although that would suck for fragmentation reasons.

Alex Pounds:        Has Anyone tried that? Have you seen anyone forking the project?

Natalie Weizenbaum: I mean, in the GitHub sense, sure, because people submit pull requests and that's how you submit a pull request. Not in the sense of trying to create a competing package manager.

Alex Pounds:        What about with Sass, are there any interesting little Sass offshoots floating around out there?

Natalie Weizenbaum: Most of the interesting Sass offshoots are implementations in different languages. No one seems especially keen on creating a different version of Ruby Sass, which is kind of unfortunate because we're winding down the support for Ruby's house and I would love to have someone step up as the official maintainer for that was, I no longer have the bandwidth for it, but so far no one's expressed interest.

Alex Pounds:        I imagined that people will start to pop up for at least companies will start to pop up once they feel the pain of the maintenance dropping away. Often, it's the free rider problem with very big open source projects that companies don't realize how much they're using something until it starts to catch on fire.

Natalie Weizenbaum: Yeah, that's totally possible.

Alex Pounds:        Does Sass get used inside of Google, I'm guessing not much?

Natalie Weizenbaum: It actually does, there are a fair number of users inside Google. Many of them are looking to move to Dart's house once that becomes more mature.

Alex Pounds:        So if there was one feature that you could take from another package manager, what would it be?

Natalie Weizenbaum: Gosh, that's tough for the most part features that I really want to see in Pub, I just implement and then they're there. One thing that I've seen in other package managers that I wish in retrospect we had done with Pub and we've kind of painted ourselves into a corner where this isn't possible anymore, is the ability to do version resolution with Git dependencies.

Natalie Weizenbaum: The way Pub works right now is that you can depend on Get, but you have to provide a specific hash, it doesn't do any kind of version resolution based on tags, which I know some other package managers do and that would be pretty cool to have, but there's not really a good backwards compatible way to do it with the way we have our dependencies set up in that, is a bummer.

Andrew Nesbitt:     And do you have a particular favorite package manager outside of Pub?

Natalie Weizenbaum: I mean when it comes right down to it, I'm a bit of a history buff so I really appreciate Bundler just for the fact that it kind of revolutionized how package management works across many, many programming languages.

Alex Pounds:        A closely related question to what features do you envy in other package managers, is what aspects of other ecosystems do you envy? Are there any other languages or setups where you say, "You know what, if only we didn't have to worry about, for instance, cross platform compatibility, we would be able to do so much more."

Natalie Weizenbaum: I would just like to see more users and more engagement honestly, I think Dart is a really rad language and I'm not just saying that because I work on it. I genuinely love programming in it, I've programmed in a lot of languages and Dart is certainly my favorite that gets used widely for production stuff. So I would like to see more people finding uses for it and creating packages and making a thriving ecosystem.

Alex Pounds:        If people wanted to learn more about Dart and Pub and maybe take their first look at the language. Where should they go to do that?

Natalie Weizenbaum: Dartlang.org it has a pretty good ramp up flow so to speak.

Alex Pounds:        And if people wanted to find out more about you and your work, where's the best place online for them to find you?

Natalie Weizenbaum: I don't really have a dedicated website right now. You go to my GitHub profile, github.com/nex3 or my Twitter account. Twitter.com/nex3.

Andrew Nesbitt:     Great. Well this has been really interesting. Thanks so much for coming on, telling us about Pub and Dart. I'm really excited to hear the outcome of your resolution algorithm changes and the potential things we can encourage other package managers to pick up in the future.

Natalie Weizenbaum: Thanks for having me.

Andrew Nesbitt:     And that wraps it up for this week. Thank you to everyone for listening. We will be back in a couple more weeks where we will try to compile down another episode and produce some more package management details for you to enjoy. Goodbye.

Natalie Weizenbaum: Bye.

Andrew Nesbitt:     Nice.
