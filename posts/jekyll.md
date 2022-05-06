
How I created and structured the newest (and last?) version of my personal website

----

The cool thing about Jekyll (and maybe other static site frameworks?) is that how it is designed for easy blogging. All the site admins have to do is upload a Markdown , HTML, or any other markup text file into a built-in directory `_posts`, maybe do some custom configurations for the page layouts and such, and *boom*, you have your website. I won't go into detail into how Jekyll works, but I will say that their [*step-by-step tutorial*](https://jekyllrb.com/docs/step-by-step/01-setup/) was very useful and easy to understand. 

<br>

# The Actual Structure

Here I describe the actual structure of the site.

<br>

## My Collections

`_posts` is an example of a collection. It's essentially a way to tell Jekyll that you want to keep a "collection" of a type of thing. These things are typically in the form of text files. Here are the collections in my site:
- projects (the collection that you see in the landing page)
- potential projects
- posts
In each collection folder (`_projects`, `_potentials`, `_posts`), I stored each item of the collection as a markup file. So each project that you see in the project list is its own file in `_projects`.

## Layouts

Jekyll uses templating with front matter (you can read more about it in their tutorial). That basically means that for each of the collection files that I write, I can put pass in some variables with it. For instance, each project would be passed along with its name, description, and time. Then in **layout files** (stored in the built-in `_layouts` folder), I can put some HTML markup to format the page. With the layout, I can tell Jekyll to make the *title* variable a certain font size or color. 

<br>

# Design Choice

My goal was to make the site minimalistic. And I think I achieved that pretty well. Right off the bat, the landing page is a list of my projects, the most important part of my site. 

<br>

I think the user interface is clean as well. No gimmicks or off-putting formatting. You have the title and any extra information on top, followed by the content. And I actually configured the CSS right where it's responsive!

----

I don't really have much to say. I largely based the file structure of my site, in terms of how I use the layouts and `_sass` files, off the step-by-step tutorial. But switching to a static site framework definitely made site updating much more easier for me. And since Jekyll is readily supported with Github Pages, I don't have to rack my brain about hosting and deployment.