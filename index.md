---
layout: page
title: Wisdom Bone
tagline: Neither wisdom tooth nor wish bone
---
{% include JB/setup %}

# Introduction

Welcome to an attempt at creating an online prescene I can control.  In the advent of social media, we have started to trade off some of our liberties for the convenience of liking status updates.  This has dumbed down our ability to express ourselves.

## One fateful day

I failed an interview, no, many interviews actually.  I have slowly crept away from the activity I used to enjoy very much, and turns out businesses rather prefer to install baked solutions written by others, than having to write these solutions themselves.  Sure, it makes sense and all, just download an open source project, build it, and then you pay no licensing costs having to operate this software for your business while making updates to the code.  At least some contributions are made back.

Despite the growing list of obscure technologies I have to deal with, this has kept me away from an important skill: writing good code.  Most of my work has been packaging software for use, but rarely have I worked on a project from its inception.

## Attempts at overcompensation

Perhaps another reason for this page is an attempt at overcompensating for failure.  In the process, I have gotten rid of my social network prescence and now only have a fledging github.  The traditional resume lacks a very important feature: an actual demonstration of skills.

Recruiters can fall victim to [SEO](http://en.wikipedia.org/wiki/Search_engine_optimization) techniques which make some candidates look better than others, prioritizing them ahead in getting interviews.  However, after understanding that I have bested the system, while failing every interview in the process, I have realized that interviews are wasting my time, and other peoples time as well.

## This is a portfolio 

It makes perfect sense.  Instead of having a list of skill keywords on a professional networking site, it may make more sense to actually demonstrate skills as blog posts.  This is just a better attempt at showing off.

## Welcome to my portfolio

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


