---
layout: post
title:  "Welcome to my Github pages blog with Jekyll!"
date:   2022-01-11 20:52:40 +0100
categories: Personal
---
Happy new year and welcome to my new blog page setup. Hope you like it! But why a new blog page? Here the short story with some technical description.

Recently during the Christmas holidays I was writing another post and reviewed the setup of my blog. Needless to say that I have not delivered much content in 2021, but I promise to be better in 2022. It was not only about the missing content, it was more about the layout and I was not satisfied with Wordpress overall too.

Then I saw an article about [Github pages](https://pages.github.com){:target="_blank"} which allows you to host your web pages directly from your GitHub repository and I started immediately testing [Setting up a Github pages site with Jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll). I liked the idea of treating my blog pages as code. Then I discovered that a Git push to Github triggers the automatic deployment and publishes of your pages. This was convincing even more.

<img src="/images/page_files.jpg"
    width="300" height="300"
    alt="Pages files in VS Code"
    style="left; margin-right: 10px;" />

The setup on Github was easy and also the first steps with Jekyll and Ruby went well. You can build your web page locally and preview at the server address <http://127.0.0.1:4000/> before pushing your changes.

<img src="/images/local_preview.jpg"
    alt="Local page preview"
    style="left; margin-right: 10px;" />

The only drawback at this time was that my blog page would then be hosted on Github with a new URL like *https://username.github.io*. After some research I found the [Deploy Now](https://docs.ionos.space) feature provided by my hosting provider IONOS which allows you to connect your GitHub account and instantly deploy your static web projects to your selected domain.

<img src="/images/deploy_now.jpg"
    alt="Deploy Now setup"
    style="left; margin-right: 10px;" />

Every time you push your local changes to the master repository on Github the IONOS Deploy Now Bot starts the workflow and deploys the web page with the latest changes.

<img src="/images/github_with_bot.jpg"
    alt="Deploy Now setup"
    style="left; margin-right: 10px;" />

If you don't want your changes to be deployed immediately after pushing to Github you could create a branch and work within the branch on your new stuff. After you completed your work, you only need to merge your branch into master and the automatic deployment will do the rest you.

Here are some additional about the [Jekyll docs][jekyll-docs]{:target="_blank"} for more info on how to get the most out of Jekyll. Find all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]{:target="_blank"}. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk]{:target="_blank"}.

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
