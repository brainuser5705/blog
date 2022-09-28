# How Heroku's end of free dynos made me questioned my motivation to code

**Disclaimer**: I know nothing about platform infastructure or IT. So if I mention something completely wrong or unaware of an easy solution to the problems I have, that is probably why.

---

## A short history

As of now, my biggest break into the programming world has been web development. I started with HTML/CSS, then started thinking about how to add dynamic content to websites, and learned all about backend development. In a nutshell, I learned about servers vs client, frameworks, HTTPs, JSON, APIs, hosting, etc. Now I would say I am pretty familiar with at least the foundational knowledge of web development. 

But one area I never trekked to far in was deployment. My go-to was Heroku, actually it was the only PaaS I ever used, and they made it dead-easy to set things up. The documentation was easy to follow and the process was so simple - all I had to do was upload my code to GitHub, add a Procfile and any other language-specific files, set up any configs through their web UI, and I could deploy my website for all the Internet to see. And it was free!

## The asteroid crashed

So when I found out Heroku was getting rid of their free dynos, I was in frenzy. Since Heroku was so easy to use, I treated all things hosting and deployment-related as a black box. I did know a thing or two about the high level concept: host a website and any other services (database, jobs etc.) on a server, point your IP address to a domain, user can access it. But that was about it. 

I spent the rest of the day and next morning, researching and thinking about the alternatives I had with the only thing in my mind being "it must be free". I contemplated making a home server but the electricity bill, lack of secured Internet, and steep learning curve wasn't appealing. I read briefly about the different levels of cloud services and PaaS/IaaS options:
- virtual machines
- cloud functions/triggers
- backend as a service
- Google Cloud App Engine (Flex)
- Kubernetes with Docket
- a lot of other things that I don't understand
And it was very overwhelming. On top of that, I felt rushed like I needed to get something up ASAP.

## The end

Right now I've just settled with continuing to use Heroku and the free student credit I get. I'm still not completely happy but I'm also not quite sure why. Maybe it's the inability to understand the process or to own the process is frustrating to me.

But regardless, this < 10 hour ordeal made me question why I was even coding. Did I code so others can see it? Was the only reason why I'm making my Spotify API project is because I wanted others to use it and see with their very own eyes that I created it? Did I feel that if it wasn't on the Internet, it wasn't worth making? I mean live demos are nice, but is that all that matters?

Back when I was (and still am) learning about web development, I wanted to get my website out as quickly as possible. After I hacked up some website (it was fun though), I deployed it, and that was when I felt the **most** joy - not the moments when I was learning and seeing results with my eyes, but the single moment when I knew that other people were going to see it.

So why do I code?

## End notes

I think this feeling is present in the other contexts as well:

![](https://media.discordapp.net/attachments/751122448543645726/1012228963856298024/IMG_1063.PNG?width=704&height=1253)

If my ephiphany does not make sense, I don't blame you. It's very introspective, but basically it shouldn't (and doesn't) matter if my code is up in the cloud or not. I code for myself and for the joy of it. So if that Spotify project doesn't show up on ashleyliew.com, well, it's fine as long as I get to see it done.