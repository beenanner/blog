---
layout: post
title = "Docker swarm mode using Docker 1.12-rc2"
date = 2016-06-25
desc: "Docker 1.12-rc"
keywords: "Docker"
categories: [Docker]
tags: [Docker]
icon: fa-code
---

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