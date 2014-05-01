---
layout: post
title: Static served content with Jekyll
description: "Migrating from PHP and MySQL driven blog to a statically served Jekyll generated website"
modified: 
tags: [jekyll, blog, tutorial]
comments: true
---
Back in 2011, my uncle told me to start a website. I had taken a class on web design in high school and knew the basics of HTML, CSS, and Javascript, but couldn't really do anything productive with those skills. During the summer, I took it upon myself to start a blog, writing it from scratch in PHP and MySQL. [Tizag](http://www.tizag.com) was the main resource I used to learn from and surprisingly (or maybe not surprisingly), those tutorials haven't changed over the past 3 years. 
\\
\\
In any case, by the end of the summer, I had put together what I thought was a successful database driven blog, full with comments and all. But it was a hassle to use. At first, posts had to be written in straight HTML. After I realized that wasn't the best practice, I tried to write some kind of markdown parser using regex, but I unfortunately was not skilled enough. Soon, writing posts became such a hassle that I just let my website sit.
\\
\\
Friends often asked me why I didn't just use Wordpress or another already well established CMS, and I always answered that it was more satisfying to do things myself. And to be honest, had I not spent the summer before college hacking away with PHP and MySQL, it's safe to say that I wouldn't have had the the same experiences developing [IlliniDumps](http://illinidumps.com) or being the Director of Information for [Engineering Council](http://ec.illinois.edu) at UIUC. But at some point, it was time to stop trying to reinvent the wheel.


### Jekyll

I was browsing the Microsoft interns facebook page and came across [this article](http://japacible.github.io/seattle-intern-guide) about the Seattle area for interns. Not only was the content useful, but I noticed the site was powered by [Jekyll](http://jekyllrb.com/). My old PHP blog had been down for a while from when I switched hosts, so it seemed like a good time to see how Jekyll felt. Having barely used GitHub and Ruby, it took me about a day to figure out how to get this all done.
\\
\\
In an ideal world, this is how updating this blog would work:

1. Create content locally on my laptop
2. When content is finished, push it to GitHub
3. Once content is pushed, the VPS will *automatically* pull down changes and deploy

The last part isn't there yet. I still have to pull down changes and deploy manually, but that's just once small extra step for now. The first steps are very dependent on your local development set up, so I will briefly go over how I set up my VPS to serve content.
\\
\\
I am using the smallest droplet available from [DigitalOcean](http://digitalocean.com) as my VPS. I host [IlliniDumps](http://illinidumps.com) on there as well, but with the combined traffic these two sites will draw, I don't think I have too much to worry about. First, you'll have to install ruby if you haven't done that already.

{% highlight bash %}
    curl -L https://get.rvm.io | bash -s stable --ruby=2.0.0
{% endhighlight %}

After that you'll need to install jekyll

{% highlight bash %}
    gem install jekyll
{% endhighlight %}

I used the [MinimalMistakes](http://mademistakes.com/minimal-mistakes/) theme, but you are free to use any (or no) theme. If you don't use a theme, you can use the following to make a jekyll project in your home directory.

{% highlight bash %}
    $ cd ~/
    $ jekyll new my_blog
    $ cd my_blog
{% endhighlight %}

Now, if you had done this on your local machine, you could run `jekyll serve` which will deploy to `localhost:4000` and you can preview your content there, but since this is on a VPS, that's not possible. You can now build your site with the following:

{% highlight bash %}
    $ jekyll build
{% endhighlight %}

Now `~/my_blog/_site` will hold your generated website. You can copy over this content wherever apache, nginx, or your HTTP server points.

{% highlight bash %}
    $ cp -a ~/my_blog/_site/* /var/www/wherever/your/http/is
{% endhighlight %}

The jekyll docs recommend to not deploy directly to your hosting directory because `jekyll build` will clean the directory before building. This means that anything in there prior to building will be deleted.

### Things to still do

I am still trying to figure out how to make pushing to GitHub automatically trigger the VPS to deploy. Once that is complete, I think I will have a good workflow for updating website. In my old site, I wrote a comment system from scratch. Again, it was a good learning experience but not practical at all. <del>Hopefully, I can figure out how to integrate [Disqus](http://disqus.com) into this blog.</del> **Done!** Other than that, playing with jekyll will hopefully open up new avenues. I never realized how big static site generation was. If jekyll doesn't float your boat, [Octopress](http://octopress.org) and [nanoc](http://nanoc.ws) seem to be popular alternatives.