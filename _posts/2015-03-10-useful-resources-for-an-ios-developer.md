---
layout: post
title:  "Useful resources for an iOS developer"
date:   2015-03-10 19:32:14
group: blog
related-posts:
- /blog/2015/08/27/a-list-of-useful-xcode-plugins
summary: There already exists great lists of resources for the iOS developer, as here and here and I also personally wrote up a list of my most used apps on Medium. However, new tools pop up all the time and sometimes it's just hard to figure out what's useful and what's not. This is my take on a list of resources for iOS developers based on my personal preferences and experiences.
---

There already exists great lists of resources for the iOS developer, as [here](http://benscheirman.com/2013/08/the-ios-developers-toolbelt/) and [here](http://dailytekk.com/2014/02/20/100-great-resources-for-ios-developers/?reading=continue) and I also personally wrote up a list of my most used apps on Medium. However, new tools pop up all the time and sometimes it's just hard to figure out what's useful and what's not. This is my take on a list of resources for iOS developers based on my personal preferences and experiences. The intention is not to create a full list of every alternative within each category, but to share resources I've either been satisfied to work with or plan to try out. I will probably update this post as I go along and get more experience in my worklife as an iOS developer. Feel free to add a comment if you feel like I'm missing something.


# Design

## [Sketch](http://bohemiancoding.com/sketch/)
I love Sketch. I used to do quite a lot of graphics back in the days when I did a lot of web development (even though I’m a rather big amateur as being the graphics guy) and also I just like to be able to do basic graphics stuff when developing. I use Sketch for designing my own apps or for checking up on sizes etc. when being handed over designs for apps.

## [Pixelmator](http://www.pixelmator.com/)
With Sketch and Pixelmator, I do not need (or use) the rather pricy Illustrator and Photoshop. I use Pixelmator for simple resizing, cropping and other asset manipulation.

## [xScope](http://xscopeapp.com/)
I am currently evaluating xScope for aligning designs provided by the designer and the actual look of the the apps I work on. So far I've been satisfied by using this app.

## [Zeplin](https://zeplin.io/)
We've been experimenting with Zeplin, where I work, to be the tool for handing over designs to the developers from the designers. It's pretty cool, as a developer, to be able to inspect all elements in the design (e.g. sizes and colours) and there's also some really interesting features on the roadmap of Zeplin.


# Code

## [AppCode](https://www.jetbrains.com/objc/)
After refusing to use AppCode many times, I finally ended up having AppCode as my main iOS development IDE. It took me some time getting use to it, but I really do feel that I’m saving time over Xcode.

## [Xcode](https://developer.apple.com/xcode/)
Even though I’m a fan of AppCode, I still use Xcode for interface building and editing project settings.

## [Tower](http://www.git-tower.com/)
My favourite git client for the mac.

## [Kaleidoscope](http://www.kaleidoscopeapp.com/)
My choice of diff tool. Worth the money? Yee.. kind of. It looks great!

## [CodeRunner](https://coderunnerapp.com/)
I never actually understood the need for using an app as CodeRunner until I figured that it’s simply just more convenient in some cases. I use it when developing/testing small snippets or when transforming Java into Objective-C/Swift.

## [PaintCode](http://www.paintcodeapp.com/)
I'm personally a big fan of having flexible assets. One way of doing this is to draw assets using code. Another big benefit of using PaintCode is that it suddenly makes it a lot easier to create custom UI components. PaintCode is an app I've been trying out, but haven't included it in my every-day toolbelt (yet).

## [CoreAnimator](http://www.coreanimator.com/#animatecode)
I've only worked briefly with CoreAnimation and everything else has been simple UIView animations. When I have the time (or need) I will look into trying out CoreAnimator as I think it looks pretty neat.


# Data management

## [Sequel Pro](http://www.sequelpro.com/)
My go-to MySQL database client app.

## [Base](http://menial.co.uk/base/)
I use Base for checking out my local SQLite database. It does the job.


# Debugging

## [Reveal](http://revealapp.com/)
My most recent app purchase which I'm starting to realise that I couldn't live without. When doing auto layout constraints in code, Reveal is really useful for debugging. AppCode comes with a built-in integration.

## [Paw](https://luckymarmot.com/)
One of my recent purchases. I used to use the Postman extension for Chrome, but I prefer to have it as a standalone app. And oh ye, I don’t use Chrome. I really like Paw!

## [Crashlytics](https://try.crashlytics.com/)
The service I've used the most for keeping track of crashes in the apps I work on. I think it is a pretty straightforward tool to use and it has been really stable.

## [Instabug](https://instabug.com/)
Bug reporting as it should be: shake the device, draw the point of attention and leave a message. I haven't used the service that much, but I've been playing around with it since the beta. I really like the approach they're taking.


# Deployment

## [Parse](https://parse.com/)
I've done backends in PHP, Ruby on Rails, Java and by using a lot different services. Being an iOS developer, I really try to avoid being caught up in a lot of backend development. That's why I tend to use Parse. It simply just allows me to do more iOS development.

## [Firebase](https://www.firebase.com/)
When I need to make apps that are more dependent on realtime communication, I've been playing around with Firebase. It's really easy to integrate and it works really well.

## [Heroku](https://www.heroku.com/)
If I end up coding my own backend, I tend to deploy it to Heroku.


# Statistics

## [Segment](https://segment.com/)
It sucks to integrate multiple SDK's and it's annoying to have to upload a new version to the App Store when you want to integrate another analytics library. Segment tries to solve this by being the single hub for collecting app usage data. I haven't used it yet, but it seems really cool.

## [Mixpanel](https://mixpanel.com/)
I used Mixpanel in a single project for analysing how the users used the app. Mixpanel provides a really clean interface making it much nicer and easier to get the overview of the usage.


# Productivity

## [Letterspace](https://programmerbird.com/letterspace/)
Normally I use Evernote for handling notes. However, I started to feel like it offered too much functionality and that's why I tried out Letterspace. Letterspace uses markdown formatting and syncs with iCloud. The interface is clean and it's possible to tag notes with # and @.

## [Alfred](http://www.alfredapp.com/)
I use Alfred as a replacement for the OSX built-in Spotlight. It works great together with Dash and I have some minor workflows I use from time to time. I also use the built-in clipboard manager, which is now irreplaceable.

## [Dash](http://kapeli.com/dash)
This app has ended up being irreplaceable. I use Dash for my personal snippets (synced using iCloud) and for reading documentation. Always googling that ReactiveCocoa method? Use Dash and save yourself some time. It is really convenient.

## [Mou](http://25.io/mou/)
When doing README files or other mark down stuff I open Mou. It’s simple and it does the job.

## [Reflector](http://www.airsquirrels.com/reflector/)
When needing to present apps, it somehow always is the case that neither an AppleTV or the proper cable for showing apps directly from your phone is present. I use Reflector to demo apps through my laptop.

## [Cinch](http://www.irradiatedsoftware.com/cinch/)
I never really understood the green OSX window button (named?) pre-Yosemite. Post-Yosemite I just think it’s a bit obtrusive. For once, I like the Windows behavior (I know..) more. Because of that, I use Cinch.


# News sources

## [objc.io](http://www.objc.io/)
High quality iOS development content published each month covering a wide range of topics such as architecture, debugging, games and security.

## [NSScreencast](http://nsscreencast.com/)
Small weekly screencasts that focuses on a wide range of iOS development topics. 

## [iOS Dev Weekly](https://iosdevweekly.com/)
A weekly newsletter with handpicked iOS development links. I like the personal touch of the author and that the majority of links are topical.

## [iOS Goodies](http://ios-goodies.com/)
Another weekly newsletter of handpicked iOS development links in categories.

## [Natasha The Robot](http://natashatherobot.com/)
The last weekly newsletter with iOS development links I'm subscribed too. The main focus of this newsletter is on Swift and that's why I subscribe to it.

## [iPhreaks Show](http://devchat.tv/iphreaks)
My favourite iOS podcast. The panel discusses a lot of different topics related to iOS development. Everything from processes, to open source and personal branding.

## [Build Phase](http://buildphase.fm/)
Another podcast on my list which I try to keep up with. The podcast is a bit more casual and loose in the agenda compared to the iPhreaks Show.

## [NSHipster](http://nshipster.com/)
Probably my favourite iOS development blog. The blog dives into small areas that are easy to overlook.
