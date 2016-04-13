---
layout: post
title: "How to: Deploy to GitHub Pages"
date: 2016-04-12 12:27:46 -0400
comments: true
categories: ["Flatiron&nbsp;School","projects"]
---
On a recent project, my team had trouble getting a working version of our single page Javascript application live. We first deployed to Heroku with a workaround using a PHP file (since Herkou is built to handle larger applications and frameworks). Our site was live, but one of our APIs used an HTTP protocol and was therefore incompatible with Heroku's HTTPS. After some research, I stumbled upon GitHub Pages which allow you to host webpages for free and uses HTTP.


#Create a gh-pages branch

Make sure the directory of the project you would like to deploy. Create a new branch called gh-pages and check it out. The branch name gh-pages is a special branch that GitHub uses to get files to build and publish from.

```
$ git checkout --orphan gh-pages
$ git rm -rf .
```
Imporant: Make sure you're in the correct branch before running the second command! This will delete all files in that directory. After running these commands, you will be ready to prepare your new files, repopulating the working tree, by copying them from elsewhere.

### Side note: What is an orphan branch?

>--orphan

>Create a new orphan branch, named <new_branch>, started from <start_point> and switch to it. The first commit made on this new branch will have no parents and it will be the root of a new history totally disconnected from all the other branches and commits.

>The index and the working tree are adjusted as if you had previously run "git checkout <start_point>". This allows you to start a new history that records a set of paths similar to <start_point> by easily running "git commit -a" to make the root commit.

>This can be useful when you want to publish the tree from a commit without exposing its full history. You might want to do this to publish an open source branch of a project whose current tree is "clean", but whose full history contains proprietary or otherwise encumbered bits of code.

>If you want to start a disconnected history that records a set of paths that is totally different from the one of <start_point>, then you should clear the index and the working tree right after creating the orphan branch by running "git rm -rf ." from the top level of the working tree. Afterwards you will be ready to prepare your new files, repopulating the working tree, by copying them from elsewhere, extracting a tarball, etc.

>Source: [git-checkout(1) Manual Page](https://git-scm.com/docs/git-checkout/1.7.3.1)

#Deploy to GitHub Pages

It's time to push your new branch to GitHub! Make sure your homepage is named index.html and located in the main directory of your repository. This is where GitHub will look for your homepage.

```
$ git add -A
$ git commit -a -m "First GH Page commit"
$ git push origin gh-pages
```

After you push to the gh-pages branch, your page will be available at: ```http://<username>.github.io/<projectname>```.


#Setting up a custom domain == "FUN"

## Step 1: Buy a domain
To host a recent project, I bought datahaiku.space for $0.88 on namecheap.com
{% img center https://media.giphy.com/media/O8pa1CyYSp1yE/giphy.gif 1000 1000 %}

## Step 2: Add a CNAME file to your GitHub repository.

Use CNAME for the file name and enter the domain you purchased in the first line of the document.

{% img center ../images/gh-pages/CNAME.png 1200 1200 %}

## Step 3: Create the necessary records for your domain

This means pointing the domain name at the IP address where your page is located. DNS is the “domain name system.” It translates human-friendly website addresses like www.learn.co into computer-friendly IP addresses like 157.166.224.25.

For GitHub Pages, you will need to create three domain records: 

1. A record for @ pointing to 192.30.252.153
2. A record for @ pointing to 192.30.252.154
3. CNAME record for www pointing to your username.github.io (username should be replaced with your actual GitHub account username)

{% img center ../images/gh-pages/DNS.png 1200 1200 %}

That's it! My most recent project is now available at [www.datahaiku.space](http://datahaiku.space/). Don't fret if your site isn't live immediately. It can take a while for the DNS to update.