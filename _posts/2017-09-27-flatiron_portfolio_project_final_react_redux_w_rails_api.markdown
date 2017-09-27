---
layout: post
title:  "Flatiron Portfolio Project Final — React / Redux w/ Rails API"
date:   2017-09-27 21:49:39 +0000
---


It’s hard to believe, but I just completed my final assignment for the Flatiron School. Both because of the scope of the journey itself and because (in the back of my mind) I know I’m not fully satisfied with this project just yet. In fact, almost always I find this to be the case with my projects.

> “The more I learn, the more I realize how much I don’t know.” — Albert Einstein

Such is the nature of learning programming; as you continue to learn the possibilities of what can be done, it can almost become daunting discerning how and what to do. In no case was this more true than my most recent endeavor with React and Redux.

While in previously, when I had what I felt was a relatively specific problem, I would generally be able to find a StackOverflow post or Medium blog regarding the topic and gain some insight. While such resources also exist for React & Redux, it seems as though many either suffer from oversimplification or have been made somewhat obsolete, (I’m looking at you React Router). Whatever the case, my final project consisted of it’s fair share of grinding with no result and minor changes leading to aha moments. After all isn’t that what coding’s all about!?

For my final act, I built out a Rails 5 API to serve as the backend for my React / Redux front-end. My project was called “JetLog” and was based around the concept of allowing a user to chart a map of diary style memories of their travels. Journaling important memories, accompanied by a location based “Log” with a photo and description of it’s meaning to them.

Initially, when planning my application, my goal was to have the majority of the screen be a Google Map culminating all the user’s memories in a visual travel footprint. Integrating the Google Maps API and hooking it up to my Logs has proved a tougher task than I was initially prepared for and one I am looking forward to ultimately completing in my continued endeavors.

In it’s current version, a user can sign-up, login and create and view their logs. While this level of functionality is certainly relatively simple, learning about all that’s involved under-the-hood of a React / Redux application truly surprised me. Progressing through building the application taught me some intriguing lessons in it’s challenges.

In particular I think perhaps the most difficult aspects of the project for me to grasp were as follows:
1) The concept of state & how Redux works
2) Containers vs Presentational Components
3) State vs Props — MapStateToProps
4) 
Having not dealt with state prior to this project, I was a bit confused. While I understand the basic idea of what state represented and why it needed to be managed, the implementation of such proved rather challenging. Initially I felt as though every component needed state and after a bit of back and forth, eventually I began to get the gist. I ultimately would reserve state for components such as login / signup forms and components that would fetch logs or routes.

Forms particularly need state of course, in order to pass their input to the Redux store and save / validate a user against it. Fetching required state in order to populate the request for logs / verifying the presence of logs and rendering routes contingent on whether a user is logged in or not.

Once I abstracted State to redux I was a bit unsure how to correctly utilize mapStateToProps. In particular I think my issue came from how obscure props seemed to me. The need to bind, the seemingly obscure nature of their requirement among other things.
