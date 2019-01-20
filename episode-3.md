Episode 3 - Rubygems with André Arko - https://manifest.fm/3
--------------------------------------------------------------

Alex Pounds:        Welcome to The Manifest, a podcast all about package management. My name is Alex Pounds.

Andrew Nesbitt:     And, I'm Andrew Nesbitt.

Alex Pounds:        And together, we're exploring the technical details of package management, the stories and the history of various projects and the communities around them too. Today we're joined by Andre Arko, the founder of Ruby Together and the lead developer of Bundler. Andre, welcome to The Manifest.

André Arko:         Hey there. Thanks for having me.

Alex Pounds:        So, do you want to explain for people who aren't familiar with Rubygems and Bundler exactly what they are?

André Arko:         Yeah, sure, absolutely. It's kind of funny because other languages have kind of like merged the concepts of package management and dependency management into the same thing, and for hilarious legacy reasons, Ruby developed those things as separate concepts and so even to this day they are two separate things. In the Ruby programming language, way back in the early midst of 2003, 2004 some great people Jim Weirich, Rich Kilmer, Chad Fowler some others got together and basically envisioned a system that would make distributing Ruby code between developers fully automated.

André Arko:         They called it Rubygems, which definitely contributed to the English language tradition of horrifying pun names for Ruby libraries. Rubygems was great, it turned everything into packages. It meant that you could say gem install Rails and you would get an entire copy of Rails and that was just this fantastic advancement on what had happened before. Fast forward maybe five or six years and so many people had Rails apps and so many people had so many Rails apps that it was very common for new developers starting out on an existing project to take days or even weeks to get that project working for them locally on their machine.

André Arko:         Bundler stepped into that void, the idea of managing the dependencies for a single application, where you can say, "These are all the dependencies that I need, these are the exact versions of all of the dependencies that I need. Now, please keep track of all of them, make sure that they all work together and allow me to install all of them with one command." I guess the Alpha and Beta versions went through late 2008, all of 2009 and we finally shipped on their 1.0 in I think late 2010.

André Arko:         The really amazing part of that is that you could check out an entire Rails app and just say bundle install and every single gem for that entire application would be installed and you could boot the app. That sounds super straight forward and commonplace and like what all of us do today just without thinking about it, but that had literally been impossible before that.

Alex Pounds:        I remember the dark times of having multiple versions of rack that would be activated at the same time as you fire up the Rails app. There was also a middle stage, I seem to remember, where Rails had some form of half-finished dependency management. Like in the, was it the application [Nabi 00:03:29], you could basically list out your gems that you needed to be configured and there was a rake task in the Rails app that would essentially just call out to Rubygems and say install all these things as well?

André Arko:         Indeed it did. It was called config.gem because there was a gem method on the application configuration object. You would list the gems that your application needed but then to find out what gems your application needed, you would need to be able to load your application. As you can perhaps imagine, it was difficult to load the gems you needed for your application to find out which gems you needed for your application and that difficulty, I think is what ultimately killed the config.gem feature.

Alex Pounds:        One thing you drew a distinction between earlier was between package management and dependency management. How are those two things different?

André Arko:         I don't know if this is necessarily true in every language but the way that it's kind of fallen out in Ruby, again for historical reasons, is that package management is the process of finding, downloading and installing code from someone else and then using that code. Where dependency management is tracking the exact code that your code needs to be able to run and managing that kind of like bundle of related and dependent code. Like a great example here is how you might install a package like say the gist gem that lets you create gists on GitHub and that's like a package.

André Arko:         It provides the gist command, it installs some Ruby code that gets run when you run the gist command and you can use gem install to install it, uninstall to remove it as opposed to application dependency management, which is what Bundler does, where you have an application, maybe a chat bot or a Rails app. And so, you would say, “My chat Bot depends on the leader gem at version exactly 1.0.3 and my Rails app depends on the Rails gem at exactly 5.0.1. Bundler not only tracks the names of your dependencies, it tracks the exact versions that you got when you installed them.

André Arko:         If you just say, “I want the Rails gem,” bundle will say, “But which version of the Rails gem do I get when I install rails? I'm gonna write down that version so that everyone else who works on this project will also get the same version of rails,” and that's kind of the essence of dependency management in a way that package management ignores. If you have an application that depends on say five or 10 different things, chances are really high that one of those things is gonna change something and no longer work with the code that you wrote against it.

André Arko:         And, that's why Bundler keeps such careful track of the exact versions of everything that your application needs to be able to run, so that you can share that with other developers and that they'll get the same results, and so that you can share it with production servers and those production servers will see the same results as you see in development.

Alex Pounds:        And, this dependency management seems to be something that Bundler has paved the way for a lot of other dependency managers that have come after it. I haven't seen many other dependency managements that existed before that had this concept of a lock file.

André Arko:         It's true. It seems to have taken hold as an idea that makes sense in retrospect. You kind of think about it and you're like, “Yeah, it doesn't really make sense to just say that you need the rack gem because what if the rack gem changes? What you need to know is the exact version of the rack gem that you need,” and before the lock file existed most of what happened was like kind of a human dependency resolution process where you would try to run the app and see if it exploded and then change some gem versions and then try to run the APP again.

André Arko:         It turns out a computer is a lot faster at that than a human, so I'm really happy with how the idea turned out and I'm actually really pleased to see that the idea of dependency management and a lock file has taken hold across the programming landscape, especially given that at the time way back in 2010 or whenever it was, Bundler was considered extremely controversial. There was a lot of pushback against this idea that you would need to bundle exec you commands. It's a huge hassle, it's not really necessary because anyone who's currently doing development has already had to work out all of the [inaudible 00:08:14].

André Arko:         And so, they have a working development environment and so what's the point of Bundler anyway? It was just this huge long running argument of like is Bundler really necessary? Existing Ruby devs don't really think so. It took a long time to change everyone's mind.

Alex Pounds:        One of the things you mentioned was that in Bundler you can specify the exact version of a package, but I've always appreciated that it also gives you some scope to leave things a little more vague. If you're installing rails, you can say you want a specific version of Rails like 4.2.7 but you can also say you want any version of 4.2 or any version 4 or even just the most up to date version.

André Arko:         That's true. It's actually one of the things that I most appreciate about the design of Bundler versus what I think of in retrospect as like prototype systems of the existing lock file. There was a gem made by someone who worked at Pivotal that I honestly think was called gem installer. It was a gem named gem installer but it effectively was Bundler except where all of the jobs that Bundler does for you today were done by a human being. You would check in a human generated lock file and then gem installer would iterate over it and install all of your gems and that does not allow you to say things like the newest version of 4.2 or the newest version of 4.

André Arko:         I feel like it's a really good balance for Bundler to maintain two explicit lists. One that's written by a human and kind of outlines the space of allowed gen versions and then one that's maintained by Bundler itself and says, “Well, out of this possible space defined by a human being, this is the exact space that we know that this code will work with and we're only gonna move within the allowed space when explicitly asked to by a developer.”

Alex Pounds:        We've had semantic versioning as a spec around for a while, but there's been a number of different implementations of a range of selectors against that semantic versioning. Was there anything around before Rubygems had actually been able to specify that idea of around version 5 or was that something that Rubygems invented as it went along?

André Arko:         You know, I'm actually not sure. My first experience with a functional high level package manager was Rubygems. I wasn't previously a pearl programmer or anything like that, so I don't actually know if package managers from older languages had what Rubygems calls the pessimistic version operator, where you can say, “Well, I want to allow up to but not including the next number.” That operator is actually much older than the semantic versioning itself as a specification. It's been really interesting to me to see how well that operator has turned out to fit in with the modern sensibility of ... well, I'm pretty sure that my code will keep working as long as say the major version doesn't change.

Alex Pounds:        That has a kind of implicit contract with the developers of those gems as well. Say if people are depending on something around version 1.2 you're kind of tied into this. I hope that the patch versions don't cause any major changes that would need me to make updates to my code that integrate with that gem.

André Arko:         Definitely. Semantic versioning has been fantastic in the sense that it has helped people standardize on a set of numbers that set people's expectations around what amount of change is intentional. But of course, the ongoing problem with computers and the reason why we all have jobs is that it's not what people's intentions are with what they do that matters. It's what we actually told the computer to do. As great as semantic versioning is, all it tells you is that the author didn't intend to make any breaking changes whether the author did intend to make breaking changes. It doesn't tell you whether or not something that from the author's perspective is a bug fix has just completely broken your application in a patch release.

Alex Pounds:        Yeah. The other aspect that comes out when using semantic versioning that is really hard to describe is as I released this new version of this library, I've made a minor change, which is to bump the version of this dependency of my library. Of course, what potentially that has done is introduced a major breaking change into the object that is now passed through the middle of my library and out into your application. But from me, as a maintainer's point of view, is a very small change that I don't consider it to have changed the API of my application.

André Arko:         That's fascinating. I feel like that's less of a problem in Ruby land due to the way that everything is in a giant name space and it's just an eval. That means that Bundler is forced to find a single version of every package that works with every other package and due to the way that, for example, node does requiring, npm is not required to find a version of a package that works with every single other package at the same time and that actually opens up pretty dramatically the possibility of what you're describing.

André Arko:         Where one package will say, “I expect an object from package foo,” and another package will say, “I provide an object from packaged foo," but it turns out that they're talking about different versions of packaged foo and in fact they are running different versions of package foo. So, the underlying code wildly fails to actually work even though technically all of the packages involved are working as intended with no bugs.

Alex Pounds:        I've seen this manifest itself where people have been using npm to bundle things for the browser and they actually end up running both Angular 1 and Angular 2 on the same webpage simultaneously. And, you can imagine that is a very subtle level of dependency hell that you've got yourself into there.

Andrew Nesbitt:     Some people would say that is one too many versions of Angular. Others would say it's two, too many. Let's take a step back and hear a little about your background, how you got into programming and how you eventually became the lead developer of Bundler.

André Arko:         My background is almost entirely just that I thought computers were incredibly neat and so as a kid and through, I guess even up through high school, I thought computers were great and amazing and I loved playing with them and I maybe tried to teach myself how to program a couple of times out of a book and failed miserably. But I made it to college and majored in computer science and finally managed to learn how to program with a teacher who knew what they were talking about rather than a book that couldn't answer my questions.

André Arko:         As I was finishing up school, I think maybe late in my junior year, I ran across this really interesting, obscure and totally useless language called Ruby and I kind of fell in love with the language itself and just thought it was fantastic to write, to read, to edit, to work with and sometime a few months after that I noticed on the Ruby mailing list, this Danish guy with a really long name announcing some web thing and I was like, “Cool, I wanted to write some web things with my Ruby,” and that was how I started using Rails and then it snowballed from there.

André Arko:         It turned out that knowing Ruby and knowing how to use Rails was actually like a wildly employable specialized skill. As a result of that, several years later, I wound up at Engine Yard as a developer working on, it was basically the product engineering team that maintained their Sass application that hosts Rails applications. At that point in time, they also happened to be paying you Yehuda Katz and Carl Lerche to work on Rails 3 and as part of Rails 3 the first version of Bundler at the same time.

André Arko:         After a few months at Engine Yard, I tried to get myself set up with a new Rails 3 app from scratch and I ran bundle install and it threw an exception. I looked into the Bundler code and it turned out, it was actually just ... it was like the equivalent of a type and I was like, “Oh, I can actually fix this.” And so I did and I sent a PR to Bundler and like a day or two later I was at work and I happened to like see Yehuda and I was like, “Hey, Yehuda I sent a PR to fix my Bundler problem. Can you accept my PR so that I can get back to trying out Rails 3," and Yehuda said, “Thanks for the PR but I'm really busy so I just gave you commit on the repo instead.”

André Arko:         I said, “Oh, thank you, I think,” and so, I merged my PR and then I saw in the issues that someone else had my same problem. So, I told them that I had fixed it and then I noticed the second person in the issues with a different problem but that also looked like it was in the same place as the code I had just changed, so I could probably fix it too. I did and then I told them that I had fixed it too. Then that kind of snowballed and a few months later Yehuda was like, “Well RailsConf wants to talk about Bundler, but I'm already giving two talks at RailsConf, so can you do the talk about Bundler?"

Alex Pounds:        Before you were programming in Ruby? What was your main language?

André Arko:         I guess I was still a student and the curriculum at my school was entirely C and C++. There was one very traditionalist professor. He would only accept homework in straight C or an Ada, but all the rest of the classes were C++ and that was actually how I wound up looking for another language because I was just sick of writing C++ and Ruby was amazing in comparison.

Alex Pounds:        When you found Ruby, what was it that you fell in love with?

André Arko:         Initially, I think it was just the syntax, being able to say what you wanted the computer to do rather than set up an obtuse and verbose set of incantations that would eventually result in the computer happening to execute the thing that you actually wanted it to execute. Then as I got more into Ruby, I got really excited about the community and about the way that the Ruby community thought it was really great to share code with each other and to tackle problems that everyone had to tackle collaboratively.

André Arko:         I mean, partly I think this was because I had been in school writing C++ and so sharing code was not forefront on my mind at the time in the 2000s but Ruby was definitely at the forefront of language communities that thought sharing open source code to solve common problems in the community was a great thing. Not very long after I started learning Ruby, RubyConf happens to be in San Diego and I was going to school just outside of LA. I attended RubyConf 2005 as someone who had just barely learned Ruby some months before that.

André Arko:         Even that opportunity to attend RubyConf when it was, I don't know, it was like 100 maybe 120 people and Jim Weirich was there with a Nintendo 64 and Ocarina of Time to run a workshop explaining how continuations works to everyone. [Matz 00:20:23] came and like proposed the stabby lambda syntax for the first time and everyone booed. Maybe 80% of Rails core was there. It was really great. Myself and two friends that I had talked into both learning Ruby and coming with me to this conference, we were able to interact directly with people that were famous cool people on the Internet.

André Arko:         We did a kind of like toy sample project while we were at the conference just implementing an RSS aggregator that we open sourced on Ruby Forge and called The R Planet. And then it turned out that shortly after that Why the Lucky Stiff made a website that needed an RSS aggregator and he used our library, it was so great. It felt like that whole experience of being able to see how great Ruby was built into the syntax of the language and then see how great Ruby was as a community is really what sold me on the overall experience.

Alex Pounds:        You joined the bundle projects at a time when it was established, but still a lot smaller than it is today. Has that growth in its usage caused any growing pains for the project?

André Arko:         It's an interesting question. I joined the Bundler team, I think while the latest version of Bundler was 0.8 and then participated in the compatibility breaking rewrite that we released as 0.9 and then, the again compatibility breaking rewrite that we released as 1.0. It was really interesting to see how everyone wasn't really sure what to expect from Bundler yet because when we shipped 1.0 it was as part of Rails 3 and instantly normalized the workflow of using Bundler and sharing code with other devs using Bundler and deploying, using Bundler in a way that I think we never could have done if Rails hadn't dependent on Bundler.

André Arko:         But it also meant that people were able to develop expectations about how Bundler worked and what it was for very quickly. That has meant that over the years, even though the number of Bundler users has grown exponentially, I think the amount of work that it's taken to maintain Bundler has grown but much more slowly than the number of users has.

Alex Pounds:        Bundler went through two rounds of breaking changes early in its life. That sounds like something that would be hard to manage and a hard decision to reach. How did you decide that those changes were necessary and how did you try and soften the blow for existing Bundler users?

André Arko:         This is maybe part of the reason why getting Bundler adoption was so contentious and such an argument at the beginning but initially, we never made any claims about the stability of Bundler. We clearly and deliberately made it a zero point something release. We said these commands and API's might change. This is basically an experiment to try and figure out what we need to do to have a good user experience. Honestly, the choice mostly came down to we want to have something that's both explainable and predictable even if you're not extremely experienced and even if you don't know what Bundler is actually doing underneath the covers.

André Arko:         We want bundle install to be a function that takes gem files as input and produces working applications with all of their dependencies as output. The previous approaches that we used for Bundler's design in 0.8 and 0.9 didn't provide that simplicity or that predictability. So, it was super rough because it often times meant that applications had to completely re-work how they loaded the dependencies again to change versions of Bundler.

André Arko:         And, it definitely got some devs completely fed up with Bundler and they said that they just wouldn't touch it until the final was out. I think Bundler's success attests to the way that being willing to throw away previous designs that didn't work really produced something quite good in the end.

Alex Pounds:        What is the elements of the design that make Bundler so reliable?

André Arko:         Definitely not how well written the code is internally. I would like to be very clear about that but from an outside perspective, bundle install is something that always does the same thing no matter what state you're starting off from. You could be starting from node gems, you could be starting from all gems, you could be starting from some older version of this application installed some of the gems but not all of them and bundle install will already run and then leave you in the state that your application needs to run.

André Arko:         I guess that level of predictability and that level of expectation about what the results of running bundle install will be is, I think the most successful part of Bundler's UX. Something that we spend a lot of time talking about in the early days as we were arguing a lot about possible designs was a principal that you could have repeated all the time while we were having these arguments and that I feel has wound up woven into the DNA of the Bundler project. It's the idea that there is no such thing as a user error.

André Arko:         If a user does something and the results don't make sense to them or the results are unexpected or the results are something that the user didn't actually want, that's a bug. That's kind of an unusual position for software to take and honestly, in 2009 or 2010 it was an even more unusual position for software to take than it is today, and being willing to say, "If this doesn't work the way the user expects, we are doing something wrong and we should fix it," helped a huge amount in producing software that today every Rubyist uses and doesn't even really think about that much.

Alex Pounds:        Do you think that willingness to bend to make the software work for the user contributed to some of the code quality issues internally in Bundler?

André Arko:         That's a good question. I think it's possible. I actually think that there's a lot of factors that contribute to Bundler being complicated, maybe sometimes over-complicated internally but I think there's probably two really big contributing factors there. The first one is that for much of its life, Bundler didn't have someone willing to say no to pull requests and even today it's a struggle. Like if there's a clear user want or a clear user need, it's very hard to say no and very easy to say yes even if the long-term cumulative effect of that is not good for users.

André Arko:         One of the things that we've tried to do as part of bringing Bundler up to 2.0 is figuring out if there are more elegant ways to solve user needs without increasing the interface complexity that users have to deal with. And, that's something that we struggle with a lot and prioritize over making Bundler's internals prettier. With the limited amount of time that we have, we would rather make the user experience better than make the internal code organization and the individual implementations of everything inside been they're prettier.

Alex Pounds:        I am pretty sure all of the users of Bundler appreciate that sentiment. There's also an implicit relationship between Rubygems, the client or the application that's running on to install these packages and Bundler? Has that been difficult to manage the changes to Rubygems that aligned with different versions of Bundler?

André Arko:         It has been a challenge, more of a challenge in the past and that it is now. There is a file inside Bundler called Rubygems Integration and it is maybe 1200 lines long and it contains all of the code needed to provide a single interface to every version of Rubygems, any interaction that Bundler has with Rubygems happens through this Rubygems integration system and it surfaces one single unified API for every operation that Bundler needs. Even though the underlying APIs that Rubygems provides have sometimes just 100% changed and have no relation across versions so that's definitely been a challenge.

André Arko:         I'm happy to say that today honestly, Rubygems and Bundler the teams have a lot of overlap. We cooperate a lot, we discuss things together a lot and in fact we've even started doing work to reduce the multiple implementations of the same thing between Bundler and Rubygems. We're kind of deliberately holding off on digging into this until after Bundler 2 is finished but ideally, what we'd like to have is a single core of gem management and then a gem command and a bundle command that simply share that core library.

Alex Pounds:        I'm glad you have mentioned Bundler 2 a couple of times as that kind of breaks the seal on the topic, what's going into Bundler 2?

André Arko:         There will be a pretty big number of changes in Bundler 2 but what we're aiming for is a transition that's pretty seamless from a user perspective even though, what's happening underneath will be pretty different than it has been in the past. Bundler core team member Colby Swandale has been pulling together a list of all of the changes but the high points are one, we're going to be dropping compatibility with older versions of Ruby and Rubygems. Bundler 1.0 came out in 2010 and that meant that we supported all of the versions of Ruby and Rubygems that were current in 2010 which included Ruby 1.8.7.

André Arko:         Today because of our policy of strict 1.0 compatibility. We still support Ruby 1.8.7 and as you can imagine, seven years on, that's actually a noticeable amount of work to keep Bundler working with ancient versions of Ruby that have been sort of abandoned or deprecated for an extremely long period of time. The other biggest sort of user facing thing that we're going to change is that we're going to stop having Bundler automatically and silently remember options that you've passed to it in the past.

André Arko:         Today if you say bundle install, dash dash, path, vendor. Bundler will actually write down path equals vendor into a settings file and if you run bundle install again without passing that option Bundler will silently act as if you had passed dash dash, path, vendor for all future commands.

Alex Pounds:        That sounds pretty useful as a feature. I'm guessing that it can cause problems.

André Arko:         Yes. So, the silent unwritten remembering of flags is definitely useful and is something that we adopted to ease adoption of Bundler across the Ruby ecosystem. But as you can perhaps imagine Bundler silently without notification applying all previous command options to all future commands without any sort of visibility into that system can be very surprising and can produce some extremely unexpected results

Alex Pounds:        Potentially even on other people's machines if that config file gets written into the repository.

André Arko:         Correct. Could you imagine, say for example, previous options that you passed to grep being applied to future commands of grep? This is not a thing that anyone running commands on the command line expects to be the case and so even though it is sometimes super helpful, we've made the backwards compatibility breaking choice to say, “You should have to actually set these settings,” and so we've adopted a system that's actually a lot more like the git command. For example, if you wanted bundle install to always install into the path of vendor. You would say bundle, config, path, vendor and then bundle install would always apply the path option with the value of vendor.

Alex Pounds:        And, that doesn't go into the gem file that goes into a separate config file, right?

André Arko:         Correct. There are a lot of config settings that are effectively per machine, like where should Bundler put the gems for this application or which gem groups should be installed or not installed on this particular machine, which can vary from developer to developer, can vary between developers and CI, can vary between CI and production. All of those config settings are per machine rather than per gem file is I think the right word, yes.

Alex Pounds:        How is the development of all of these features for Bundler 2 actually funded?

André Arko:         Well for the last about two years now, what we've been doing to try and sustain and fund development work on Bundler and on Rubygems, I started a non-profit called Ruby Together. It's the type of non-profit known in the US as a trade association. Companies and developers pay the trade association to be a member and then the trade association takes that money and turns around and uses it to pay the developers to make the Ruby open source developer tools that everyone who uses Ruby needs and benefits from and Ruby Together pays for the work to keep those working, to make those better, to add features, fix bugs, answer support tickets et Cetera, et cetera.

André Arko:         After two years, Ruby Together is doing pretty well. We're able to afford to pay a handful of developers for about five hours per week worth of work. For me that five hours a week usually goes to high level project management and what direction we want to go with Bundler features, Rubygems features, how those projects work together and for the individual developers who work on the projects themselves. It's things like answering support tickets, writing new features, improving documentation, installing new software updates on the servers et Cetera, et cetera.

Alex Pounds:        What was the real need for Ruby Together?

André Arko:         Well, the real need was that no one was keeping any of this software working. At the point that I started Ruby Together, I was pretty much the only person actively working on Bundler or Rubygems combined and there was only one volunteer system administrator left for rubygems.org who actively engaged in kind of day to day maintenance. To be clear, there are always people who are willing to step up in an emergency and when we have had emergencies, we have always had community stalwarts step up and provide the help that we need to get everything sorted out, but many of those emergencies are completely avoidable with a small amount of daily grind week after week maintenance and that is the work that ultimately was not getting done.

André Arko:         And so, Ruby Together was started with the specific idea that Ruby devs and Ruby companies need that work to be done even if they maybe don't realize it at the time, and the only way to ensure that it gets done is to pay regular developer market rate for the work.

Alex Pounds:        What kind of emergencies have come up?

André Arko:         Since Ruby Together has been paying for ongoing maintenance? I'm happy to say that there have not been any full outages of rubygems.org for the entire two years that Ruby Together has been paying for maintenance. Prior to that, some memorable outages include the time that Bundler DDoSed rubygems.org I think it was only down for about one day that time but bundle install was extremely slow for maybe a week while we rebuild the Bundler API in a way that it wouldn't take down the main service.

André Arko:         Maybe a year or two before that, there was a really notable downtime where there was a security vulnerability that no one had time to fix right away and someone figured it out and used it to paste bin the Etsy passwords file from rubygems.org We actually had to burn the entire server and then we had to checksum every single .gem file in S3 to make sure that the hacker hadn't replaced any of them.

Alex Pounds:        Where did you get the checksums from to be able to do that?

André Arko:         Mirrors that we knew were taken before the hack happened.

Alex Pounds:        Right, because you can't trust your own copies.

André Arko:         Correct. Various hosting providers had internal mirrors. There were a few regional mirrors and we collected all of the mirrors that we could find together and across all of them we were able to find at least one and almost always two copies of a checksum for all but maybe like the most recent 20 gems that had been pushed before the hack and so, we just deleted those 20 gems and said, “We're really sorry but you need to upload a new version.”

Alex Pounds:        And, did you find any one's which had been compromised?

André Arko:         In fact, none had been compromised. The hacker appeared to be doing it as a lesson to us about patching our servers rather than with malicious intent.

Alex Pounds:        Wow, that sounds like that was a lot of work to fix that problem.

André Arko:         Oh my God, it was so much work. It burned out one rubygems.org system administrator. He effectively never felt up to working on it again and of the three or four of us who were there at the time, I think each of us took at least one and maybe two or three unpaid days off of work to work on the servers and rubygems.org was still down for a week.

Alex Pounds:        So now that Ruby Together exists, everything is better.

André Arko:         I would say things are more stable. They're definitely better from my perspective. I think that having security patches applied promptly is worth paying for and I think having developers who know that they will spend time making sure that these tools work with the next version of Ruby is also worth paying for.

Alex Pounds:        Why did you choose to set it up as a trade organization?

André Arko:         I guess I had initially thought that I was simply going to start a regular corporation but as I talked to a lawyer about the possibility of incorporating in the US, this lawyer super luckily, happened to have worked with some trade associations in the past and said, “Hey, it sounds like the goals of your corporation actually line up very strongly with this non-profit idea of a trade association. I didn't know this at the time but open source projects actually have a decent amount of history with trade associations because of the way that they line up.

André Arko:         In the US the definition of a trade association is a non-profit that receives funds from people in an industry but produces benefits that benefit everyone in the industry not just the people who provide the non-profit with funds. So, that lines up really well with the open source model, where we pay developers for work using member's funds but even Ruby developers who aren't members benefit from the work that those developers do on the open source tools that we support.

André Arko:         Other well-known examples of trade associations include the Linux Foundation, the JavaScript Foundation and the American Dairy Farmers Association, which uses funds from farmers all across America to pay for ads that say, "Got Milk."

Alex Pounds:        I was hoping you would say that use funds to develop a milk package manager.

André Arko:         That would be amazing.

Alex Pounds:        What's the relationship between Ruby Together and Ruby Central like?

André Arko:         It's pretty good. I guess I should back up a little bit. Ruby Central is a different kind of nonprofit. They are a five 501(c)(3) charity mostly because they were simply founded long enough ago that the IRS was not yet suspicious of companies asking to be charities because they were involved in software. Sometime around 2012 the IRS said, “Hey, wait a second, we can't just let anyone who gives away software stop paying taxes,” and ever since then it's been basically impossible to get a status of charity if you're a software project.

André Arko:         But Ruby Central, as a charity, is responsible for administering the RubyConf International Ruby Conference and the RailsConf International Conference and the other thing that Ruby Central does is they pay the AWS bill for the servers that rubygems.org runs on, which is super great. It means that we don't have to worry about raising money for that. Those funds come from funds raised by selling tickets and sponsorships for those conferences and so effectively those two conferences foot the AWS bill for the servers and mean that everyone can talk to those servers, upload gems, download gems and everything's happy.

André Arko:         The system administration work, the DevOps work, the feature development, bug fixes all of those things done by developers that's all paid for by Ruby Together. We have a monthly coordination meeting where Ruby Central shows up, Ruby Together shows up, the actual system administrators show up and we kind of like synchronize, make sure that we're all on the same page, make sure that the work's getting done. It works out pretty well.

Alex Pounds:        What are some of the challenges or resistances that you've faced as you've put together this collection of people that are essentially acting on behalf of the Ruby community.

André Arko:         Definitely the biggest challenge that we've faced is convincing companies that they actually want to give us money. It's a little hard to describe concisely but in essence, Ruby companies accurately realize that if they don't give us any money, they will continue to receive all of the benefits of everyone else giving us money and this is a classic tragedy of the shared common situation, where any work that we do benefits everyone and so no one is motivated to ensure that we're able to continue to do the work.

André Arko:         I shouldn't say no one. Stripe in particular, has been the strongest and most steadfast supporter of Ruby Together for its entire existence and actually the CTO at the time, Greg Brockman, came up with the entire idea and just came up to me and said, “Hey, if Stripe gave you money, would Bundler be better?” And I said, “Wow, yes. Is this a trick question?” And he just responded, “That's fantastic. Stripe will give you some money then and we'd love for Bundler to be better."

André Arko:         And so, with no expectations and with no meddling from above, Stripe has just consistently funded Bundler and Ruby Together and made sure that there is at least a baseline amount of money for us to be able to do some work every month and we were able to take that baseline level of funding and start the entire non-profit based on that original situation.

Alex Pounds:        Presumably, you wouldn't recommend this approach to other open source projects looking for funding. They should not just wait for a wealthy and generous benefactor to appear and offer them a giant wheelbarrow full of cash.

André Arko:         I definitely do not recommend it. It seems like a once in a lifetime kind of thing to me and I do not expect it to ever happen again.

Alex Pounds:        So, what do you see as the future of sustaining open source software or funding open source software?

André Arko:         Ultimately, we're going to need to figure out some way to normalize the idea that companies need to contribute back to their infrastructure or that infrastructure is gonna collapse. Ruby Together is definitely a specific attempt based on the exact circumstances of Bundler, and of Rubygems, and of myself. The most interesting attempt that I've seen at generalizing this kind of approach is the project called Open Collective. They basically provide the infrastructure for any project, meetup, group or open source project to say, “Hey we want to collectively raise money but then pay it out to meet our collective goals in a transparent way."

André Arko:         As you can maybe imagine Ruby Together has had to do a bunch of work itself. We had to incorporate, we had to come up with our own structure, we had to come up with our own corporate rules, we had to come up with our own system for accepting payments, we had to come up with our own system for paying payments and Open Collective, which has come into existence in a post Ruby Together world is kind of a productification of a Ruby Together style situation.

Alex Pounds:        Does being a trade organization put any kind of restrictions on the things that you can offer to your supporters?

André Arko:         It does and interestingly enough, the restrictions that come with being a trade association were things that I actively wanted to build into Ruby together, even when I thought it was going to be a company. So it's fantastic to have the full weight of the IRS behind these requirements. The biggest requirement is that we are not allowed to sell anything to companies that give us money. So for example, I've heard a lot of people say, “Well, why don't you sell private gem hosting to raise money?”

André Arko:         And, on the one hand, my experience leads me to believe that selling private gem hosting would not actually be a worthwhile way to pursue funding. But on the other hand, our non-profit status explicitly says that we can't sell services to customers so that's not even an option. But it actually reassures me to know that even in some worst case hypothetical future where Ruby Together is going on entirely without me, it's a requirement that's built into the fabric of the company itself that all of the work that Ruby together does must be available to anyone who uses Ruby whether they can afford to pay Ruby together or not.

Alex Pounds:        That has a lot of parallels with the GPL and Copyleft licenses, I feel that idea of a Ulysses pact that you've put in place, especially with Ruby Together that basically says upfront, “These are the things that we're not going do later on with the explicit ties that won't let you change your mind if the offer comes along later.” Andrew, how do you define a Ulysses pact?

Andrew Nesbitt:     I'm probably gonna get this completely wrong. A Ulysses pact comes from Greek mythology where Ulysses was going to be sailing past the Bay of Mermaids. The mermaids would sound wonderful but they would eat you if you sailed too close to them. And so, he got his whole crew to kind of tie themselves to their boat before they sailed near the mermaids so that they couldn't untie themselves and change the direction of their ship as soon as they started to hear the Mermaids. Which basically meant that even though they wanted to change their mind later on, they had no way that they could untie themselves and change the direction of the ship after the fact.

Andrew Nesbitt:     In the same way that if you open source your code under the GPL license, it doesn't allow you to go back and take away that code and say, “This code can no longer be available to everyone,” and any future changes that are built on top of that must also follow those same kind of rules and anything that once it's been distributed under a license must continue to be under that license.

Alex Pounds:        So, this is in no way relevant to open source but do you know off the top of your head how Ulysses crew manage to free themselves after they had passed the danger?

Andrew Nesbitt:     I like to think that they just sailed in a straight line and ended up gently arriving at port and someone untied them at the other end but to be honest, I don't know.

André Arko:         My understanding of the story is that some of the sailors plugged their ears with wax and that any sailors who did not plug their ears with wax had to be tied to the mast of the boat and then even though they begged their shipmates to untie them, it was too late.

Alex Pounds:        If somebody wants to learn more about Bundler, where should they go for that?

André Arko:         We maintain a website of pretty documentation at bundler.io If you're interested in getting involved in Bundler, there is a guide to contributing to the Bundler project itself. If you browse to the Bundler repository on GitHub, it's under the Bundler Organization so that's Bundler/Bundler. There is a file named contributing.md inside the docs folder and that file contains a guide to joining our project Slack, looking for issues where you can start to contribute, sending PRs to fix typos, documentation errors at useful comments that sort of thing.

André Arko:         And, if you're really interested in getting deeply involved, I've both done a conference talk that's up on YouTube and written a blog post version of that talk about how to get involved in open source projects and how to join the core team of those projects with as little investment as 15 minutes per day of your time and honestly, in open source, if you keep contributing over time, sooner or later you'll be the only one left and that means you'll be in charge.

Alex Pounds:        You say that like it's a good thing.

André Arko:         I don't know if it's a good or a bad thing. I think it's just the truth. Open source is people doing stuff for free and eventually people's lives will interfere with their doing stuff for free and so if you're willing to do stuff for free more than anyone else, you'll get to make the decisions about what happens.

Alex Pounds:        Then if somebody wanted to make a case for their company to support Ruby Together. Is there a good place they could find information about that?

André Arko:         There is, there's a website at rubytogether.org talking about the work that we're doing and why we feel like it's important and there is a page specifically targeted at companies talking about the benefits that we offer to companies who are members and the benefit that companies receive from our work at rubytogether.org/companies.

Alex Pounds:        Fantastic. Well, thank you so much for coming on and talking to Andrew and me about Rubygems and Bundler and Ruby Together. It's been really interesting to find out some more of the backstories and hear about the early days of Bundler and how it got its start.

André Arko:         Yeah, absolutely. Thanks for having me on. It's been a pleasure speaking to both of you.

Alex Pounds:        And, that's it for this week. We'll see you again in a couple of weeks' time.

André Arko:         Awesome. Thanks so much.
