---
title: "How I created this website you see before you"
date: 2022-07-18
slug: "website"
description: "A guide on how I created and deployed my website"
keywords: ["guide", "website", "google", "domain", "warwick", "university", "student"]
draft: false
tags: ["project"]
math: false
toc: true
---

## Motivation

For the first project on my blog I thought it would be fitting to go through the process of how I got this website setup and running. I created this website using Hugo, GitHub, and Netlify. If you do not want a custom domain, you can avoid using Netlify and you can just use [GitHub pages](https://pages.github.com/) instead.

## Prerequisites

Creating the website in the way I did requires no previous knowledge, there are some excellent tutorials out there which I will reference that will guide you through the process. At the start of each subsection I will provide some brief, bulleted steps. I would encourage you to still watch the video as this will ensure you do not miss anything.

Anything that needs downloading will be linked in the bulleted steps where appropriate. Aside from that all you should need is a GitHub account which is very simple to setup.

## Development

### Initial setup

* [Install Hugo](https://gohugo.io/getting-started/installing)
* Create your site folder
* Pick a [theme](https://themes.gohugo.io/) and follow the instructions provided by the author
* Modify the site and test that the changes are outputted on your local server

{{< youtube c7vpcqA6SEQ >}}

### Using Netlify

A note on the final point. I struggled at this step as I firstly created a repository for my whole desktop meaning there were 10,000+ changes mentioned in the source control. To fix this I found the culprit folder by typing `git rev-parse --show-toplevel` in a terminal directed to your site folder. Then navigate to that folder, show your hidden files and you should be able to see it. You can then delete this and ensure you only have the git repository in your site folder.

The next issue I had was that I created a different repository by accident so when I was committing changes it was not working. To fix this I simply started using the repository I had accidently created. Genius I know.

* Push your local repository to the GitHub website with [GitHub Desktop](https://desktop.github.com/)
* Depending on how you implemented the theme all the content may get moved first time, or you may have to do the steps in the video [5:45]
* Login to Netlify with your GitHub and link your website repository
* Test you can make a change within Visual Studio Code, push it to GitHub (on the website), then have it be deployed on Netlify

{{< youtube hBQlCtfRmqs >}}

### Getting yourself a custom domain (optional)

* Obtain that domain!

{{< youtube S7DVyHfv4zM >}}

### Linking Netlify and your custom domain

* Find the IP from the [documentation](https://docs.netlify.com/domains-https/custom-domains/)
* Setup the custom records
* Wait about 5 minutes
* Apply certificate if it was not done automatically
* Check everything is working! 

{{< youtube Q9giWrfIJKk >}}

## The final result

Go and explore it yourself!

## Reflection

Although I did not code the website from scratch I was still able to gain an appreciation for the processes involved. Moreover, I was able to use GitHub more which was beneficial as I would like to become more comptetent at it since it is so widely used in industry.

I intend to continue adding blog posts to this website, and hopefully I will be able to modify it further to make it truly my own. Stay tuned!
