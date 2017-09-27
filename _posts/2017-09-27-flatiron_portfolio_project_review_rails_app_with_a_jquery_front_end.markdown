---
layout: post
title:  "Flatiron Portfolio Project Review — Rails App with a jQuery Front End"
date:   2017-09-27 21:39:09 +0000
---

Long before I began my web development journey, I remember how easily fascinated I was by the dynamic aspects of the web. From moving logos, to endless scrolling sites, I’ve always had a markedly more memorable experience interacting with such websites.
Fast forward to today, and my appreciation for such elements has only grown. Having just implemented jQuery into my previous Ruby on Rails app Branch (which you can read about here), wasn’t quite as difficult as building out the app itself, but did have it’s challenges. Below is a walkthrough of the jQuery features as well as an overview of the project itself.

**Here are the Specific Project Requirements and my plan to how to implement them in Branch:**

**1. Must render one index page via jQuery and an Active Model Serialization JSON Backend.**
With Branch being a community events aggregator, allowing for comments was a natural progression. With the introduction of jQuery, I was able to render a branch’s comments index directly on the page per user request.
Branch show pages include a “Load Comments” link that populates a div with relevant comments without requiring the user to leave the relevant branch.

**2. Must render one show page via jQuery and an Active Model Serialization JSON Backend.**
Within the cities index page, I wanted to be able to tease / preview the categories belonging to it on a hover. I felt this would optimize the User Experience — informing users of the categories currently populated in a particular city prior to making a selection.

**3. The Rails API must reveal at least one has-many relationship in the JSON that is then rendered to the page.**
In the index page, a City has many Categories.
In the show page, a Branch has many Comments.

**4. Must use your Rails API to create a resource and render the response without a page refresh.**
This meant that for example, if a user submits a comment to a branch, the comment is serialized and submitted via an AJAX POST request, and appended to the DOM without having to refresh the page.
Within the Branches show page, you can create a new comment and it will immediately appear in the comments div, alongside all other comments on said branch.

**5. Must translate the JSON responses into Javascript Model Objects. The Model Objects must have at least one method on the prototype. Formatters work really well for this.**
This requirement meant that the JSON responses must be turned into JavaScript objects prior to being appended to the page. Upon submission of a comment, the id and content of said comment was used to create a JavaScript Comment object. Doing so allowed for a reproducible object that could itself have it’s own methods. Thus, I was able to abstract out the DOM targeting and appending of a comment into a postComment() function.
