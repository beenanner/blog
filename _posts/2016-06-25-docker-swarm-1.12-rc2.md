---
layout: post
title = "Docker swarm mode using Docker 1.12-rc2"
date = "2016-06-25T12:00:00-04:00"
desc: "Docker 1.12-rc"
keywords: "Docker"
categories: [Docker]
tags: [Docker]
icon: fa-code
---

<!DOCTYPE html>
<html lang="en-us">
	<head>
		<meta charset="utf-8">
		<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
		<meta name="viewport" content="width=device-width, initial-scale=1">
		<meta name="author" content="Jonathan Lee">
		<meta name="description" content="My place to share ideas and discoveries!">
		<meta name="generator" content="Hugo 0.15" />
		<title>Docker swarm mode using Docker 1.12-rc2 &middot; My Personal Blog</title>
		<link rel="shortcut icon" href="http://beenanner.github.io/images/favicon.ico">
		<link rel="stylesheet" href="http://beenanner.github.io/css/style.css">
		<link rel="stylesheet" href="http://beenanner.github.io/css/highlight.css">
		<link rel="stylesheet" href="http://beenanner.github.io/css/monosocialiconsfont.css">
		
		<link href="http://beenanner.github.io/index.xml" rel="alternate" type="application/rss+xml" title="My Personal Blog" />
		
	</head>

    <body>
       <nav class="main-nav">
	
	
		<a href='http://beenanner.github.io/'> <span class="arrow">←</span>Home</a>
	

	
		<a href='http://beenanner.github.io/about'>About</a>
	

	
	<a class="cta" href="http://beenanner.github.io/index.xml">Subscribe</a>
	
</nav>

        <section id="wrapper">
            <article class="post">
                <header>
                    <h1>Docker swarm mode using Docker 1.12-rc2</h1>
                    <h2 class="headline">June 25, 2016</h2>
                </header>
                <section id="post-body">
                    

<p>I recently returned from Dockercon 2016 in Seattle. The announcements of new features and improvements were many, but one  in particular has me very excited. With the release of Docker 1.12-rc2 they introduced Swarm mode! This allows for native orchestration and service discovery without the need of any third party tools.</p>

<h3 id="tools:cecf0c1be2aaa9d1893b8b123a42ad3c">Tools</h3>

<p>You need a few tools installed to follow along with this example.</p>

<ul>
<li><a href="https://www.docker.com/products/overview">Docker for Mac or Windows</a></li>
<li><a href="https://www.virtualbox.org/wiki/Downloads">VirtualBox</a></li>
</ul>

<p><br /></p>

<h3 id="create-a-few-boxes-for-our-cluster:cecf0c1be2aaa9d1893b8b123a42ad3c">Create a few boxes for our cluster</h3>

<p>Here we are just using docker machine to create 3 virtualbox that will house our swarm.</p>

<ul>
<li>docker-machine create -d virtualbox swarm-manager</li>
<li>docker-machine create -d virtualbox swarm1</li>
<li>docker-machine create -d virtualbox swarm2</li>
</ul>

<p><br /></p>

<h3 id="create-the-swarm-cluster:cecf0c1be2aaa9d1893b8b123a42ad3c">Create the swarm cluster</h3>

<p>Here we create a manager which will be able to schedule container to run on the swarm as well as a couple of nodes to participate in the swarm connected to the manager.</p>

<p>The following will setup a swarm cluster on the 3 boxes previously created.</p>

<ul>
<li>eval $(docker-machine env swarm-manager)</li>
<li>docker swarm init &ndash;listen-addr $(docker-machine ip swarm-manager):2377
<br />
<br /></li>
<li>eval $(docker-machine env swarm1)</li>
<li>docker swarm  join $(docker-machine ip swarm-manager):2377
<br />
<br /></li>
<li>eval $(docker-machine env swarm2)</li>
<li>docker swarm  join $(docker-machine ip swarm-manager):2377</li>
</ul>

<p><br /></p>

<h3 id="create-a-service-in-the-swarm:cecf0c1be2aaa9d1893b8b123a42ad3c">Create a service in the Swarm</h3>

<p>Here we will just spin up the default nginx container.</p>

<ul>
<li>eval $(docker-machine env swarm-manager)</li>
<li>docker node ls</li>
<li>docker service create &ndash;name nginx -p 80:80 nginx</li>
<li>docker service ls</li>
</ul>

<p><br /></p>

<h3 id="scaling-a-service-in-the-swarm:cecf0c1be2aaa9d1893b8b123a42ad3c">Scaling a service in the swarm</h3>

<p>Let&rsquo;s try to scale our nginx container to 5 spread across the swarm.</p>

<ul>
<li>docker service tasks nginx</li>
<li>docker service scale nginx=5</li>
<li>docker service tasks nginx
<br />
<br />
This is a lot of info. We created 3 virtual machines. Created a swarm. Spun up a nginx container somewhere on the swarm. Finally we scaled the nginx service to 5 containers. There is so much more you can do with the new swarm mode and this is just the tip of the iceberg. I will try to post more on docker as I become more familiar with the new features.</li>
</ul>

<p>*Disclaimer: since this was introduced in a release candidate the final commands and API could change before being fully released.</p>

                </section>
            </article>
            <footer id="post-meta" class="clearfix">
                <a href="https://twitter.com/beenanner">
                        <img class="avatar" src="http://beenanner.github.io/images/avatar.png">
                        <div>
                            <span class="dark">Jonathan Lee</span>
                            <span>“Do. Or do not. There is no try.” - Yoda</span>
                        </div>
                    </a>
                <section id="sharing">
                    <a class="twitter" href="https://twitter.com/intent/tweet?text=http%3a%2f%2fbeenanner.github.io%2fpicks%2f06-25-2016%2f - Docker%20swarm%20mode%20using%20Docker%201.12-rc2 by @beenanner"><span class="icon-twitter"> Tweet</span></a>

<a class="facebook" href="#" onclick="
    window.open(
      'https://www.facebook.com/sharer/sharer.php?u='+encodeURIComponent(location.href),
      'facebook-share-dialog',
      'width=626,height=436');
    return false;"><span class="icon-facebook-rect"> Share</span>
</a>

                </section>
            </footer>

            <div id="disqus_thread"></div>
<script type="text/javascript">
    var disqus_shortname = 'beenanner';
    var disqus_identifier = 'http:\/\/beenanner.github.io\/picks\/06-25-2016\/';
    var disqus_title = 'Docker swarm mode using Docker 1.12-rc2';
    var disqus_url = 'http:\/\/beenanner.github.io\/picks\/06-25-2016\/';

    (function() {
        var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
        dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
        (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
<a href="http://disqus.com" class="dsq-brlink">comments powered by <span class="logo-disqus">Disqus</span></a>

            <ul id="post-list" class="archive readmore">
    <h3>Read more</h3>
    
    
        
        <li>
            <a href="http://beenanner.github.io/picks/06-25-2016/">Docker swarm mode using Docker 1.12-rc2<aside class="dates">Jun 25</aside></a>
        </li>
        
   
    
        
        <li>
            <a href="http://beenanner.github.io/picks/04-27-2016/">Picks of the Week (04/27/2016)<aside class="dates">Apr 27</aside></a>
        </li>
        
   
    
        
        <li>
            <a href="http://beenanner.github.io/picks/03-31-2016/">Picks of the Week (03/31/2016)<aside class="dates">Mar 31</aside></a>
        </li>
        
   
    
        
        <li>
            <a href="http://beenanner.github.io/picks/03-26-2016/">Picks of the Week (03/26/2016)<aside class="dates">Mar 26</aside></a>
        </li>
        
   
    
        
        <li>
            <a href="http://beenanner.github.io/picks/03-17-2016/">Picks of the Week (03/17/2016)<aside class="dates">Mar 17</aside></a>
        </li>
        
   
    
        
        <li>
            <a href="http://beenanner.github.io/picks/03-13-2016/">Picks of the Week (03/13/2016)<aside class="dates">Mar 13</aside></a>
        </li>
        
   
    
        
        <li>
            <a href="http://beenanner.github.io/picks/03-06-2016/">Picks of the Week (03/06/2016)<aside class="dates">Mar 6</aside></a>
        </li>
        
   
    
        
        <li>
            <a href="http://beenanner.github.io/picks/02-29-2016/">Picks of the Week (02/29/2016)<aside class="dates">Feb 29</aside></a>
        </li>
        
   
    
        
        <li>
            <a href="http://beenanner.github.io/picks/02-21-2016/">Picks of the Week (02/21/2016)<aside class="dates">Feb 21</aside></a>
        </li>
        
   
    
        
        <li>
            <a href="http://beenanner.github.io/post/web-dev-position-available/">Web Development Position Open<aside class="dates">Feb 17</aside></a>
        </li>
        
   
</ul>
            <footer id="footer">
    
        
<div id="social">
    
    <a class="symbol" href="https://www.facebook.com/beenanner">
        circlefacebook
    </a>
    
    <a class="symbol" href="https://www.github.com/beenanner">
        circlegithub
    </a>
    
    <a class="symbol" href="https://www.twitter.com/beenanner">
        circletwitterbird
    </a>
    
</div>

    
    <p class="small">
    
        © Copyright 2016 Jonathan Lee
    
    </p>
</footer>

        </section>

        <script src="//ajax.googleapis.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>
<script src="http://beenanner.github.io/js/main.js"></script>
<script src="http://beenanner.github.io/js/highlight.js"></script>
<script>hljs.initHighlightingOnLoad();</script>


    </body>
</html>
