# Lamp Stack

---
*(This is a repost)*. How I designed the previous version of my website, explaining the custom backend structure and inherent flaw with it (which is why I ended up scraping it)
---

## Database Entities
If you look at the navigation bar, you will see the three main components of my website: *projects*, *status updates*, and *blogs*. One feature I was keen on having was the ability to readily insert any of these entities into my website instead of manually updating, for instance, an HTML file each time. This requires a database and an admin site. The admin site would serve as the interface for me to insert the content with predefined SQL queries. I was able to achieve this goal and even automate some parts (I will eventually have to scrap this design which I will explain why later). 

## Templating
On every page of my website, you will see that it has the same header and footer. The pages are also structured: for example, the status updates are aligned vertically and always followed with a timestamp. Rather than hardcoding each page and mix the backend code with the frontend code, I created a template system. If you are familiar with the Django framework, the system is very similar to the concept of views and templates. I have a colllection of templates for each of my page, as well as templates for the layout of each entity in the page, and each page's folder has an `index.php` which serves as the view. The `index.php` file is what you see every time you load a page. It uses SQL queries to get appropiate data from the database, uses the template to insert the data in the right spot and returns the HTML that you see.  

## Deployment
I am hosting the website with Heroku as a PHP application with the PostgreSQL addon database they provide. I won't explain the whole process of eployment with Heroku. All you need to know if that you can link a Git/Github repository (where all the code for your website is stored) to Heroku and tell Heroku to deploy it. Heroku will build everything from the linked repository and provide a URL for your deployment.    
  

Here is where I will have to scrap my design. Whenever I create a blog, it will automatically create folder containing all the blog's files, essentially changing the codebase. However, it only updates the current Heroku deployment, not the linked Github repository. So everytime I make changes to the code and deploy with the Github repository, Heroku will take a fresh clone of Github code. That means all the files that were automatically created in the previous Heroku deployment are gone (Josh W. Comeau describes a similar issue with using third-party servers in his [blog](https://www.joshwcomeau.com/blog/how-i-built-my-blog/) under "Downsides"). As a result, I had to remove the automated processes in my code and now have to manually commit the folders/files with Github. 