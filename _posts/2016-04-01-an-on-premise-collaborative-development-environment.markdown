---
layout: post
title:  "An On-Premise Collaborative Development Environment"
date:   2016-04-01 8:05:58 -0500
permalink: /2016/an-on-premise-collaborative-development-environment/
---

In this post, I'm going to share my thoughts about an on-premises private cloud 
configuration for collaborative software development. These are just some
conceptual notes, but I'll try to double back later and provide step-by-step 
directions for setting up the system I'm going to describe here.

The substrate I prescribe is Joyent's Triton, which is completely open source, 
so a finished product can be deployed to the Joyent public cloud or in a 
colocated datacenter. I won't go into depth about standing up Triton, because 
[Joyent has graciously described how to stand up Triton][1] on affordable 
consumer hardware.

As a footnote, Triton was originally known as SmartDataCenter, so you'll see 
many references to `sdc` in the context of Triton. Just build a mental regex.

**UPDATE:** I didn't mean to spread FUD! While the open source version of 
Triton does include an [operations portal][8], it's worth checking out the 
(also) open source [sdc-portal][2] developed by 
[Faithlife](https://faithlife.com/). Although it hasn't been updated in a 
while, Faithlife's sdc-portal has some very compelling features and I'd 
encourage anyone interested in deploying Triton to check it out.

A somewhat little known fact about Triton is that it comes with 
[an LDAP server][3], which provides a perfect hook for integration with [GitBucket][4]. GitBucket is an open source clone of GitHub and by integrating 
with sdc-ufds (the "Unified Foundational Directory Service"), we can revoke all 
privileges in the system through the sdc-portal interface. Pretty sweet, huh?

It gets better. Triton exposes the entire datacenter as a single `DOCKER_HOST` 
with [sdc-docker][5], so you can develop *and deploy* containerized 
applications without any painful virtual machine configuration. With Triton, all
of your containers will be running at bare metal speeds and provisioned across 
all available compute nodes. It isn't actually Linux, but it's close enough.

Can we put some icing on this cake? Okay.

How about a [multi-user version][6] of [Jupyter Notebook][7] for collaborative, 
explorative development? The beautiful part about this is that once you have 
Docker configured on your machine to connect to your private Triton 
`DOCKER_HOST` you can just do some variation of:

`docker run -d --name jupyter.hub jupyter/jupyterhub jupyterhub`

That's pretty much it, although there are several approaches that can be taken 
here for authentication (including another LDAP integration). Jupyter is no 
longer limited to Python, so you can also use Ruby, R, and several other 
languages if you so choose. You are almost certainly going to want to build 
your own custom docker image for Jupyter Hub to support your goals, but this is 
a pretty simple process. Again, I'll try to provide more detail in another post.

**Why would you bother with any of this?** Well, the only money you'd spend on 
this environment would be on hardware that you'd own. It's not exactly 
expensive to develop on the Joyent public cloud or host your private 
repositories on Github, but if you consider the total cost over the lifetime of 
a business these things can add up pretty quickly. When you start to run the 
numbers, there is a very real economic argument in favor of this approach.

That's the general outline! Just food for thought.

[1]:https://www.joyent.com/blog/spin-up-a-docker-dev-test-environment-in-60-minutes-or-less
[2]:https://github.com/Faithlife/sdc-portal
[3]:https://github.com/joyent/sdc-ufds
[4]:https://github.com/gitbucket/gitbucket
[5]:https://github.com/joyent/sdc-docker
[6]:https://github.com/jupyter/jupyterhub
[7]:https://github.com/jupyter/notebook
[8]:https://docs.joyent.com/private-cloud/install/operations-setup