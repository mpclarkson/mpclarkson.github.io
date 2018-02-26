---
layout: post
title: The Daily Spin
comments: true
---

Well it's been a while since I updated this blog, and a lot has happened since.

I've moved away from hardcore dev and back into the marketing world. However, I'm focused on technical marketing, including data analytics, so I get to use Python quite a bit now.

One thing that hasn't changed is my passion for cycling. I'm still regularly pumping out 250km+ per week, and loving every minute of it.

while I'm interested in it, I suck at keeping up to date with cycling news. There's just too many great publications to keep on top of and, often, the content I want is buried deep. My favourite source of cycling news is [Cycling Tips](https://cyclingtips.com) but I still find it hard to keep on top of their great content.

So, over the weekend I decided to build a web app that aggregates cycling news from all the top global publications to make it easy to find the content that I'm interested in. This is something I've been thinking about doing for a long time, so I just fucking did it!

The site is called [The Daily Spin](https://dailyspin.com/) and it currently aggregates news, articles and reviews from about 15 different sources. Articles are pushed into Elasticsearch and indexed to make searching across topics and publications really simple. The index is updated every 15 minutes so it's pretty current.

It's meant to make it easy to browse great cycling content.

Currently you can just browse by keywords and categories but I'm considering functionality such as, trending topics, trending articles, suggestions, upvoting/downvoting content etc, following topics etc.

Let me know your thoughts! It's a very early alpha product but (so far) it seems pretty robust.

The tech stack is:
- Django 2
- Python 3.6
- Elasticsearch
- Redis
- Postgres

Ride on!








I've been keeping an eye out for projects looking to develop robust web frameworks in open source Swift.

A week or so ago I stumbled on the very promising [Vapor](https://github.com/tannernelson/vapor) - a Laravel inspired web
framework written in Swift. Check it out and star the repo.

You can build a basic Vapor web app really easily using [this installer](https://github.com/mpclarkson/vapor-installer)
I wrote over the weekend.

Download the binary from the repo, add it to your `$PATH` enter `vapor new ProjectName` at the command line (OSX or Linux)
and there you have it - a blazingly fast Swift web app.

Happy coding.

Matt