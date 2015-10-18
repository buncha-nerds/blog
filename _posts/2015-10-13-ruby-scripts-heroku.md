---
layout: post
title: Running Ruby Scripts Periodically on Heroku
---

If you've ever needed to run a simple ruby script periodically, say, to check something and then send a notification out, then there's good news! You can do this for *free* with [Heroku](https://heroku.com)!

-----

# The Story
Recently, I wrote a ruby script to check to see when PAX badges are available. Basically, it checks for any mention of badges through official channels, and then pushes out a notification to me as soon as it detects a new mention. It is designed to just simply be run in the command line. The problem is, I would need to have it constantly looping and running on a machine, or it would have to be kicked off periodically on a local machine.

I didn't want to have to run a machine all day just to loop a script or have cron kick off a script occasionally. So, I needed to look for a solution that would have some other service host and kick of my script for me periodically.

Here's where Heroku comes into play - you can create a Heroku app to hold a ruby script, and use the [Heroku Scheduler](https://scheduler.heroku.com) to kick off scripts in set frequencies for you!

# How to do it
+ Create your script keeping in mind that it will be run periodically
+ Create a new heroku app
  + Install the heroku-toolbelt if you haven't already
  + Run `heroku new` in the local git repository that contains your script
+ Configure any necessary environment variables critical to your script. For example, API keys and tokens.
+ Add the Heroku scheduler add-on to your app. This is easiest to do from Heroku dashboard and the [Scheduler](https://elements.heroku.com/addons/scheduler) website, but you can do it in the command line as well by running `heroku addons:create scheduler:standard` ![Add Scheduler](http://buncha-nerds.com/blog/assets/2015-10-13-ruby-scripts-heroku/scheduler.png)
+ Push your app to Heroku using `git push heroku master`
+ Schedule your job
  + Find the Heroku Scheduler add on in your app's dashboard on Heroku ![dashboard](http://buncha-nerds.com/blog/assets/2015-10-13-ruby-scripts-heroku/scheduler-access.png)
  + Or, run `heroku addons:open scheduler` to automatically open it
  + Enter the command you would use to run your script in the Heroku scheduler and set the desired frequency (Scheduler supports every day, every hour, or every 10 minutes) For example, I run my PAX badge script with `ruby pax-badger.rb east 600` (the arguments specify looking for PAX East badges specifically, and letting the script know that it's going to be run every 600 seconds, or 10 minutes) ![heroku scheduler](http://buncha-nerds.com/blog/assets/2015-10-13-ruby-scripts-heroku/heroku-scheduler.png)
+ You can check the output from your script by checking the logs! Run `heroku logs --tail` to see it in your console.
+ Of course, you can always make changes to your script whenever your want and then push it to Heroku!
+ profit!
