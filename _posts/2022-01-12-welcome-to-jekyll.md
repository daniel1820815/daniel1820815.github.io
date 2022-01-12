---
layout: post
title:  "Welcome to my Github pages!"
date:   2022-01-12 11:17:00 +0100
categories: Personal
---
Happy new year and welcome to my new blog page setup. Hope you like it! But why a new blog page? Here the short story with some technical description.

Recently during the Christmas holidays I was writing another post and reviewed the setup of my blog. Needless to say that I have not delivered much content in 2021, but I promise to be better in 2022. It was not only about the missing content, it was more about the layout and I was not satisfied with the either current Wordpress template nor with Wordpress overall too. I wanted to have a simple and flat design without any further modules and add-ons which I have to manage and integrate. I want to focus on content in 2022.

Then I saw an article about [Github pages](https://pages.github.com){:target="_blank"} which allows you to host your web pages directly from your GitHub repository and I started immediately testing [Setting up a Github pages site with Jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll){:target="_blank"}. As I am very focussed on Network Automation these days I liked the idea of treating my blog pages as code. After some reading through the docs I discovered that a Git push to Github triggers the automatic deployment and publishes of your pages. This was convincing even more and I started to rebuild my blog page in VS Code.

<img src="/images/vscode.jpg"
    alt="Pages files in VS Code"
    style="left; margin-right: 10px;" />

The setup on Github is good explained and easy to deploy. Also the first steps with Jekyll and Ruby went well. The Jekyll documentation was not so easy to follow for me, but there is a tutorial which I recommend to go through. This will help you to understand the basics and you can evolve on top of that.

In less than an hour I have build the first skelton of my new blog page. My pre knowledge with HTML and experience with Markdown helped a lot to make fast progress. Now I spent a lot of time with getting to know all the various options and features to fine tune. There is also a very helpful option to build your web page locally during developing and preview at the server address <http://127.0.0.1:4000/> before pushing your changes.

<img src="/images/local_preview.jpg"
    alt="Local page preview"
    style="left; margin-right: 10px;" />

The only drawback at this time was that my blog page would then be hosted on Github with a new URL like <https://username.github.io>, in my case <https://daniel1820815.github.io>.

After some research I found the [Deploy Now](https://docs.ionos.space){:target="_blank"} feature provided by my hosting provider [IONOS](https://www.ionos.de){:target="_blank"} which allows you to connect your GitHub account and instantly deploy your static web projects to your selected domain.

<img src="/images/deploy_now.jpg"
    alt="Deploy Now setup"
    style="left; margin-right: 10px;" />

Every time you push your local changes to your master repository on Github the *IONOS Deploy Now Bot* starts the workflow and deploys the web page with the latest changes.

<img src="/images/github_with_bot.jpg"
    alt="Deploy Now setup"
    style="left; margin-right: 10px;" />

If you don't want your changes to be deployed immediately after pushing the repository you could create a branch and work within the branch on your new stuff. After you completed your work, you only need to merge your branch into master and the automatic deployment will do the rest you.

Take a look at my [Github repository for this blog pages](https://github.com/daniel1820815/daniel1820815.github.io){:target="_blank"}. I will try to focus now more on the content of my blog, but alongside I will take a look at the options of Github pages and Jekyll. I hope you enjoyed this short side trip. Please reach out to me via email or Social Media if you have any questions. You will find all details below the post in the footer.

Here is some additional information about [Jekyll docs][jekyll-docs]{:target="_blank"} for more info on how to get the most out of Jekyll. Find all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]{:target="_blank"}. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk]{:target="_blank"}.

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
