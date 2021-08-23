Episode 16: Conda Forge, Mamba, and Packaging Con with Wolf Vollprecht - https://manifest.fm/16
--------------------------------------------------------------

Alex Pounds:
Welcome to The Manifest, an occasional podcast all about package management. My name's Alex Pounds.

Andrew Nesbitt:
And I'm Andrew Nesbitt.

Alex Pounds:
And together, we're talking with folk from the world of package management. We're exploring the technical details, hearing the stories and history of their projects, and learning about the communities around them too.

Andrew Nesbitt:
Today, we're joined by Wolf Vollprecht, the creator of Mamba, a package manager that's part of the Conda ecosystem. Also, a core member of the Conda-forge project. And one of the organizers of the upcoming packaging-con, a virtual conference all about package management. Wolf, welcome to the podcast.

Wolf Vollprecht:
Hello.

Andrew Nesbitt:
So Wolf, you've done a lot around the scientific packaging space. Do you want to give us the overview and how you got started in doing Mamba, working on Conda-forge and these related projects?

Wolf Vollprecht:
Yeah, so I started at the company called QuantStack in Paris after I finished my master's studies. And they were already using Conda and specifically, Conda-forge to distribute the software that we were working on. And the main software that we were working on at that time, or I was working on, was the software called Xtensor, which is a scientific library, similar to NumPy, but in C++. As the company, basically, we were also involved in maintaining quite a few other packages on Conda-forge for the scientific ecosystem. Especially, we are very active in the Jupyter community, which is a interactive computing platform. It's distributed over PyPI and Conda-forge.
And so we are maintaining quite a few of the packages for the Jupyter ecosystem over there, and that's how I got involved in the Conda ecosystem, let's say. I'm not sure if all the listeners are familiar with Conda. Conda is a package manager that is cross platform and cross language. So it's like APT in the sense that there are binary packages, or DNF on Fedora. It's like a Linux package manager, let's say, but it also works on Windows and macOS. And most people think that Conda is something specific to Python, but it's not true. It's actually managing native binary's for different programming languages and most prominently C/C++ obviously, but also Fortran as a possibility, and even Rust or Lua or Go are distributed as Conda packages.

Alex Pounds:
So why would people choose to use Conda for their package management over one of the other language specific or operating specific package managers?

Wolf Vollprecht:
Many language specific package managers don't distribute binary packages, I would say. It's not true for all of them and for example, Python has recently got this capability of shipping wheels, which are pre-compiled, but for a long time, basically before Conda was created, I think the Python package manager for example, was not shipping binary files. And obviously there's a lot of power if you have a cross language package manager, because it makes it much easier to combine different packages from different ecosystems or languages. And also the way maintenance of the packages works is different in the Conda-forge community, because with a language based package manager, you usually have the person that writes the source code, upload the package and with Conda-forge it's more like Linux distribution in the sense that you have a specialized packaging community that takes care of the packages themselves.

Andrew Nesbitt:
And does that make it easier to distribute complete systems for the package manager where you can say, "Oh, I want to pull in these Python packages for my program, but I'm also going to need say Redis on the side as well."

Wolf Vollprecht:
Yeah. You can have exactly that. You can combine Python and Redis easily because you can just install them as packages and Conda makes other things easy or nice because it has a real SAT solver to solve for package dependencies. But you can also create environment files to recreate accurately the environment from one computer to another one. And you can pin down versions to basically arrange or exact versions that you know that they work. So those are features that maybe not all language specific package managers even have.

Alex Pounds:
So Conda forge is a community run project. Is it complimentary to the Conda registry that is maintained by the people that run Conda?

Wolf Vollprecht:
Conda-forge is in some ways completely independent from AnaConda. So AnaConda is the company behind the Conda package manager and AnaConda also has a website called anaConda.org, which has different channels for the Conda package manager. And the channel is similar to a PPA, let's say in the Ubuntu world, basically a collection of packages. AnaConda hosts those packages and Conda-forge is one channel on the AnaConda org platform. But there's AnaConda people helping out in the Conda-forge community and the AnaConda channel basically also takes recipes from the Conda-forge community. So there's an exchange between AnaConda the company, and Conda-forge as the community project around the Conda package manager.

Alex Pounds:
Out of interest Wolf, do you know how big the Conda repository is?

Wolf Vollprecht:
It's over 10K packages at least. And if you're talking about size and terabytes, then the last time I checked, it's three and a half. But just to know that I'm talking about the Conda-forge channel here. So there other channels that are also large or have lots of packages.

Andrew Nesbitt:
Do you know roughly how much bandwidth gets taken up per day?

Wolf Vollprecht:
I actually don't, but it's quite a lot. And it's also difficult to reliably run the servers and we see downtimes. And there's also a CDN involved and the cloning step to the CDN so that the packages are close to the users and all of this. And it's quite complex, and luckily this is all taken care of by AnaConda, the company that runs this for us as the community. But yeah, it's definitely a lot of work and engineering that test to go into and doing this.

Andrew Nesbitt:
How does this lead into Mamba as a re-implementation of Conda?

Wolf Vollprecht:
Conda-forge as a channel on AnaConda was growing quite fast and large. And one of the unique things about Conda-forge is that we are never deleting packages. So the packages are just there forever, and that makes the channel even bigger. Because with other Linux distributions, for example, you reset the state at certain points and you only have fewer packages because you make a large release every year or so. But with Conda-forge we want to enable, for example, reproducible signs. So packages should stay there forever, and the SAT solvers should take care of figuring out the upper bounds of package versions to install and stuff like this.
And this growth has led to some problems with the Python implementation of Conda, and especially at the solving step, which was getting slower and slower. And basically I had this idea of trying to use another library called Libsolve, which is also used in the Linux world, for example, in the DNF package manager. And I wanted to explore if we could use Lipsolve to solve these Conda dependencies. And that started this entire Mamba development where we try to use Lipsolve and started to re-implement things in C/C++ because Lipsolve is written in C and it's really fast. And gradually, we also re-implemented basically everything else that necessary for the Conda package management experience in C++. And that has quite some advantages in terms of speed. But the biggest advantage really is to use Lipsolve.

Andrew Nesbitt:
I'm curious about the cross compatibility here in the sense that you've got the official Conda implementation, which has one particular solver, and you have the new Mamba implementation, which is using a different solving engine. So presumably they can come up with different valid combinations of packages, depending on which one you're using. Does that play into the compatibility question? Is it a case of, if you produce a different log file with different versions defined, that's completely fine. Or do you ever hit any issues where it's like, "Oh, the Python version produces one answer and the C++ version produces another answer, and that breaks that compatibility somehow."

Wolf Vollprecht:
As you mentioned, there's the possibility of finding different solutions and I've compared quite a few Conda resolutions with Mamba resolutions or Lipsolve, let's say. And yes, they are sometimes different, but usually both are valid. It's just that they go into different local minimus and I think that's expected and fine. And we haven't seen major problems for quite a long time now with the resolutions that Mamba gives.

Alex Pounds:
In the initial stages of experimenting with Lipsolve, did you consider contributing Lipsolve as a solver directly to Conda?

Wolf Vollprecht:
Absolutely. I did consider that, but at the time there was quite a bit of worry in the Conda community because they have to support very old installations and have to be backwards compatible for a long time. And starting Mamba made it much more easy to experiment with new features. Also, the solver and Conda is somewhat plugable, but not exactly at this point. Right now there's a enhancement proposal going to make it more pluggable and integrating this same solver as Mamba has. But at the time when I started, it was not really easy to integrate this different solver into Conda itself. And starting a different project, had some benefits, especially around quick experimentation and breaking things. Also, the Conda release cycles are quite slow and all this led to this different path.

Alex Pounds:
Yeah, that makes sense. There's a lot of cruft that comes with trying to contribute new features to existing package managers. And I mean, I'm surprised that Conda even is able to move towards more modular solvers. There are so many package managers that the solver is just splattered everywhere throughout the code base and is very difficult to extract from everything else. Because it's all just to go up organically, it's like ivy wrapped around a house and if you take it down, then you pull the house down at the same time. I guess you're primarily focused on the scientific community at the moment?

Wolf Vollprecht:
Yes, we're quite focused on the scientific community, but really Conda and Mamba are both super general solvers or package managers. And there's nothing specific towards the scientific ecosystem let's say, and it could just be used in any other context as well. We found pretty good adoption in the Conda-forge community of Mamba, but we're also trying to be really friendly and not being bad citizens or trying to get people to move from Conda to Mamba or something like that. But people with large environments or complicated installations usually have seen quite dramatic benefits of using Mamba over Conda.

Andrew Nesbitt:
So Although member doesn't have anything specific to it's use in those scientific computing circles, do the scientific computing users have specific concerns or requirements? What makes a package manager more suitable for use in that programming environment?

Wolf Vollprecht:
I think the scientific use in this case is defined by the packages that are available, and with Conda-forge, we just have a large scientific community that is creating packages and maintaining them for the Conda and Mamba package manager. And getting the benefits of easily distributing the results for cross platform usage so that the students can use their MacBooks or can use the Linux computers or run it on their HPC clusters, basically.

Andrew Nesbitt:
Another community oriented thing you're involved with is packaging-con, the first installment of a conference all about package management. How did you get into organizing that?

Wolf Vollprecht:
It all started on Twitter when Todd and I were discussing basically differences with Spack and Conda. And we were kind of thinking both that it would be nice to have a conference where we could all come together, basically. All different package managers that exist currently and discuss our problems and our solutions and how we manage communities and build scripts and all this kind of stuff.

Alex Pounds:
There doesn't seem to be a huge amount of sharing work between different communities on how to build and maintain package management. Was that one of the driving forces behind getting these people together?

Wolf Vollprecht:
Yes, absolutely. I think so too and I'm really quite saddened by the fact because package management is really hard. And for example, writing the scripts that work for different operating systems is also something that's really hard. And often in package management, we have to patch a certain packages to make them compatible with whatever underlying operating system you have and stuff like this. And sharing this knowledge and breaking down these silos would be so great because it's just a lot of wasted effort in a way to have all these different approaches. And maybe we can find some common ground to bring all these communities together and see if there are things that we can develop to maybe give everybody a similar metadata format or things like this to make everyone benefit basically.

Alex Pounds:
Yeah. Try and get that shared understanding, another area that again, the podcast has been really good for is sharing those war stories where if you're not in that community, as it goes through the growing pains of Conda-forge getting so large that SAT solving becomes painful. When you land in a community that already has a package manager that's been through all that pain, it's not clear what had to happen and the pain the users and the maintainers had to go through to get out the other side of that with a working solution.
Sharing some of those stories is a really good way of getting everyone on the same page, but also leaving some breadcrumbs and some warning flags, maybe for people that come in fresh thinking, "Oh, it must be easy to build a package manager or at least a fun thing to do." And then end up going like, "What have I done? I am now maintaining this thing and it's growing as more users come on board, but it's also now very hard to change because it's being used in projects and depended on as a piece of infrastructure."

Wolf Vollprecht:
Yeah. And one of the challenges that you have when you're building a package manager is for example, how to do security and how to do package signing and that's one thing that we're working on with Mambo. And this is a lot of work that goes into doing this. And it's a wasted effort to do this over and over again, because if we have some common libraries to do this for us, to solve package management in a cross-platform way, that would be really nice to help all ecosystems, basically. And every language having to come up with their own package manager, it seems to be a bit of a waste of effort that could be better maybe spent to go into language development or other fun things.

Alex Pounds:
What do you think causes that insularity? Why do these individual package managers end up working on the same problems over and over again and not sharing their solutions?

Wolf Vollprecht:
That's a difficult question. I think we can see different types of package managers. One type is the Linux distribution that comes with a package manager and that the package manager is a bit defining I guess, for the Linux distribution. And then we can also see that most programming languages tend to create their own package manager and they are across platforms. So the package manager runs on Windows and macOS and Linux, but it doesn't work very well for any other programming languages. Those are the two types of package managers. And then there are also the source package managers and somehow it just seems like a problem that did not yet find perfect all encompassing solution, so that people seem to need to write their own package managers. And that leads to the siloing because of reinventing the wheel in a way.

Andrew Nesbitt:
So I have some opinions on why this happens. I don't necessarily have any solutions. One that stands out to me is that often the language package managers fit the particular structure or features of the runtime that it's built for. So you could take npm and the JavaScript world. JavaScript has support for first class functions, so you can... Not necessarily, you should, but you can require multiple versions of the same package in your runtime without them colliding and exploding. You can't do that in say Python or Ruby, because when that module gets loaded, it goes into the global namespace. And if you try to load another version, the language would say, "No, I can't do this," different languages have different features that can be mapped onto some features or particular quirks of certain package managers. At least at the language level... Also distros are usually put together curated by people that didn't write all of the packages that go into them.
So they're packaging up an end product for the users of the operating system, rather than individual authors of the packages publishing them out. You end up with language package managers often self-serve, like there's no restrictions to publishing, you don't need permission from someone else. You just grab a free name or free namespace and publish, and then it's available to be instantly installed by whoever. With a new language you get the opportunity to start a new namespace and kind of, "Oh, I can reuse these names."
Whereas if you tried to reuse, say a SP library in Conda-forge for Ruby, I'm going to have to make up a name that fits with all the existing HTP libraries that are in Conda-forge for every different other language, and you end up with some pretty verbose Ruby dash version number of Ruby dash HTP client, or similar. So it doesn't have quite the same ring to it as being able to go, "Oh, well, we've got our own package manager and my library is called HTTP or similar," creates a slight amount of tribalism, but also as a new language often, that is like people are jumping to that because it's new and it's fresh.
And it gives them the opportunity to start over and do a bit from scratch similar to you saying, cleaning out from an old version of a distro is like, "Oh, well, we can actually clean up some of this old stuff." Starting a new package manager ends up being a little bit like that as well, which is not helpful for the community at large and for enabling this cross language kind of sharing. But I think that's one of the big factors in why we keep seeing the languages just starting from scratch because they get some benefits of doing that and breaking away. But they also lose out by not being able to make use of existing tooling and lessons learned from other communities necessarily.

Wolf Vollprecht:
Yeah. I think I totally agree on all points that you mentioned. And I still think that maybe packaging-con can be this point where we come together and maybe try to find solutions for this. One of the interesting things that we've started off for packaging-con was to get an overview over different, let's say access for package managers. Maybe on the one end of the spectrum, you'd have a binary package manager and on the other end you have a source package manager. So that could be one comparison access, and another one could be whether it's programming language specific or whether it's cross language, whether it works on different operating systems or not, et cetera. So the idea was to have a talk about these different categories, and then get lots of package managers to show what their specific package manager does and try to get really an overview over what's there and what people are doing. And that will hopefully be one of the key events at packaging-con to give everybody a idea of what's happening in the whole world of packaging software.

Andrew Nesbitt:
I'm nodding along here because getting that taxonomy down a shared language for different package managers so that you're able to see like, oh yes, we actually do agree on this approach. Or we consider this to be one of our key features or drivers of the design of our package manager allows you to group them. Even if two package managers might not seem very similar at all, because they're used for different languages, which are used for different businesses, being able to look at them like, "Oh, well these fit into this mental model," in a similar way means that they can have a discussion regardless of what the packages that are being published are used for. Which is a good way of breaking out of some of that siloing effect.

Alex Pounds:
My gut feeling here is that another contributor to this is the classic programmer hubris of thinking, "Oh, I just need to install some packages. How hard can that be?" Right? You're creating a new language. You want to be able to distribute your dependencies as part of that. So easy, just create a new package manager. You just need to pull down a few other packages and unzip them, basically pop them in the file system, job done. And it's only when you start looking into this and having that community engagement, you start realizing, "Oh, dependency resolution is an entire field of study all on its own."
Reproducibility is another one. The cross-platform support, the security, the signing of packages, the ability to support reproducible builds, but also allow people to say, "Oh, I've accidentally packaged up all of my production credentials. I need to yank that package," that I think is something which people don't really see until they're really deep into this project. Or they've started tugging on this thread the entire jumper has unraveled.

Andrew Nesbitt:
And on the top of that, you might end up running some pretty large servers serving a lot of bandwidth. You're just trying to keep those things from catching on fire all the time. Any changes to the server might be breaking changes to the client. So it can be really hard to fix any early mistakes or things that you wished you could change can be really difficult to steer the ship once it started to move.

Wolf Vollprecht:
And back the topic of collaborating more. I think one thing that might be missing a bit is cross-platform library to do package management. Which Lipsolve for example, is trying to achieve. But Lipsolve was also Linux only before we started making it working on windows and macOS. And maybe getting more libraries to do the grunt work let's say, of package management. That could be an interesting thing to come out of packaging-con.

Alex Pounds:
So what talks do you have lined up already for packaging-con?

Wolf Vollprecht:
Actually, not so many and we are still hoping to get many more. There are people from the Conda-forge community that are planning to give a talk. But hopefully all topics around packaging, obviously, like package management, dependency management, software supply chain attacks that we've seen recently and pipelines how you can build packages, how we are writing build scripts. For example, with Conda-forge we're using CI pipelines and we have one way of formatting metadata, but hopefully we can see other ways of taking care of metadata of packages. We're super interested in reproducibility, software release methodology. For example, if you're doing ACS releases or distros, or if you have a rolling release, all these kinds of questions we really hope to see lots of inspiration coming on in different talks. And we really invite everyone to submit talks that fit into this really wide category of package management.

Alex Pounds:
What do you think are the unsolved parts of package management? There's a lot of areas in this where different languages take different approaches and you have different solutions and different solvers, and they might take different paths, but they all get to a valid solution. But what are the areas where we haven't figured out the correct answer yet, or the way to solve certain problems?

Wolf Vollprecht:
For me, the biggest problem is the metadata of packages and especially the dependencies. Usually the metadata for packages is written by humans and it's often the dependencies are imperfect because we cannot really know the future evolution of other libraries that are used by my package. And so defining upper bounds for the versions or SO versions of other libraries that are consumed by my package, that's pretty hard. And I think there are lots of small holes in most people's metadata. And it could be really interesting to have an automatic system to find these upper bounds or incompatibilities between packages and package versions.

Andrew Nesbitt:
Yeah. This is a really interesting area to me as well. We had Sam on talking about Go and we didn't dip into it too much, but Go is then settled on it's minimum version selection method, which is a hack taking on that problem. But it pushes the problem out rather than being like, "Oh, well, we won't worry about the SAT solver trying to get through this." Instead it requires the maintainers to be actively bumping and making sure that they're describing if their package works with all of the dependencies that they have when major versions come out. And it half tackles that problem, but also completely misses the mark. Because as you mentioned, doing something automatically would enable that to continue working when someone say stops, maintaining a particular library.
That gets stuck if you don't have a human in the middle there right now. So I don't think they've solved it directly, but at least they've taken a slightly different approach and it's an interesting experiment. I'm not sure if it's worked out particularly well, Go people would probably say it's worked out well for them, but I get the feeling that a lot of packages have basically stopped bumping major version numbers to avoid the whole system, as their way of working around that rather than properly describing all of their dependencies in a complete way.

Wolf Vollprecht:
Yeah. And that topic would be something really interesting to see what the Go people figured out and get their insights at packaging-con for example, that will be great. And we're really excited to foster this exchange of ideas. I might be able to tease a talk that comes out of the Conda-forge community here, where one of our community members has worked on extracting symbols from all the packages that are available on Conda-forge and put them into a large database. And hopefully at some point we can use this database to figure out when was some symbol removed or some function definition changed and things like this to get more interesting metadata that we can use to figure out compatibilities and maybe enable better metadata.

Alex Pounds:
The packaging-con call for papers is currently open. What kind of speakers are you looking to attract?

Wolf Vollprecht:
We'd really like to attract developers of package managers and packaging community people to speak and share their experiences, their lessons learned and show us what cool things they are working on and inspire everybody learn from each other.

Andrew Nesbitt:
And people who should have lots of experience already and be dab hands at giving talks. Right?

Wolf Vollprecht:
Not at all. You could be a absolutely newcomer to conferences and all of that. And we hopefully can make sure that you will feel very welcome and we're more than happy to have new people coming in.

Andrew Nesbitt:
And this is a virtual conference. Conferences aren't really happening in person, again, due to the pandemic. But was this something that you thought would make a good virtual conference or would you really have liked to be able to do this in person?

Wolf Vollprecht:
I would have loved to do this in person, but I think running this as a virtual conference also gives us some opportunities, because it makes it much easier for people to attend and the cost of attending a new conference that hasn't happened before is very low. So hopefully we have a nice turnout, but obviously there are some challenges and one decision that should make it easier for speakers, and hopefully also more interesting for conference goers is to have the talks live. So there is not going to be prerecorded talks and also we work with a foundation called NumFOCUS, which is running a lot of conferences these days, and they will help us with their expertise to make this a really nice experience for everyone, hopefully.

Alex Pounds:
And obviously you're missing the hallway track of an in-person conference. How are you planning to get people to discuss the talks after the videos have been published? Where do you go from there to continue the conversation and keep that community going and thinking about the kinds of problems that they will have been exposed to during the conference?

Wolf Vollprecht:
Yeah, that's really the difficult part with virtual conferences, to recreate the hallway track. We're going to use one software where you can work around with an avatar and meet the other people in the virtual space. We're not quite sure how well this is going to work, but hopefully we can figure out a way to have open rooms where people can just join and start discussing with other attendees.

Andrew Nesbitt:
And do you think that this could become a regular event? Is there enough content to keep doing say a yearly packaging content? Does there are enough people doing enough new and interesting things in package management every year?

Wolf Vollprecht:
So at the first meeting we had was the organization committee for packaging-con and everybody already started diving into the details of the different package managers, and package management solutions and started discussing how they're doing things. And that was really great to see basically, that there's so much energy in this community and interest in learning about the problems and solutions of others. I think package management is a really big topic and it's a really important topic. And it has a lot of impact for many companies and systems and everybody basically who was running software on their devices. And so I could absolutely foresee it to become a regular conference and that's also the plan, and I really hope that there's enough content to keep going with this.

Alex Pounds:
So if people want to learn more about Conda-forge and Mamba, where's the best place for them to go for that?

Wolf Vollprecht:
The best place to learn about Conda-forge is the Conda-forge website at Conda-forge.org. And to learn about Mamba currently, it's the read-the-docs page or the GitHub repository. And you can also join us on our Gitter channels. Gitter is a chat platform. You can just log in with your GitHub account and go to the QuantStack lobby, where you can discuss about Mamba or the Conda-forge lobby to discuss about Conda-forge. But you can also ask questions on the Conda-forge channel about Mamba and I will be there.

Alex Pounds:
And we'll get links to those in the show notes.

Andrew Nesbitt:
And what about you? Where should people go to learn more about you?

Wolf Vollprecht:
You can always find my GitHub profile at Wolfv and there's my email, you can shoot me an email.

Alex Pounds:
And lastly, packaging-con, where should people go to learn more about attending? And if people were interested in submitting a talk, where should they go for that?

Wolf Vollprecht:
We have a website at packaging-con.org. And there are two buttons to register as an attendee and to submit a talk. We've got a bit more information and the deadline for the submissions for the talks is going to be on the 31st of August. So I hope this is enough time for everyone to prepare.

Alex Pounds:
Well, this has got me very excited. I'm really looking forward to packaging-con. I might even submit a talk myself for some stuff that I've been experimenting with recently around packaging and content addressing. Thank you very much for coming on and sharing it and look forward to definitely attending the conference and helping to get more knowledge sharing out into the package management world is an excellent reason for putting on an event like this.

Wolf Vollprecht:
Yeah, thanks for having me, that was really great. And if anybody in the community has ideas, what we should do at packaging-con, then also feel free to contact me and tell me what you think we should be doing. And we really look forward to this event and I think we're all excited to talk about our different solutions and all of that.

Alex Pounds:
Thanks very much and goodbye.

Wolf Vollprecht:
Bye-bye.
