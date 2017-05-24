---
layout: post
title:  Off the Rails: RoR Project Review - Problems with Nesting, Routing & More
date:   2017-05-24 20:18:29 +0000
---

“How do you learn?”. A question that many of us have grown to become familiar with, yet, perhaps something we struggle to identify outside of the context of the process itself. Of course, while learning itself is a reflective act, it seems ironic to think how seldom we may find ourselves questioning how we do so. During my Rails project, I found myself questioning how I would learn something that seemed insurmountable, and in doing so reaffirmed my appreciation for the process of learning itself.

As I began my project, I was excited at the prospect of expanding upon my previous efforts for my Sinatra Portfolio Project: Branch, a community events aggregator. Making the switch to Ruby presented both exciting opportunities for expansion and challenges I had yet to meet in prior learning. I integrated such gems as Devise and Omniauth for user creation and authentication, as well as Pundit, for establishing admin and base-user behavior roles. Rails additionally allowed me to create nuanced model properties, that while perhaps possible in Sinatra certainly would not have been nearly as intuitive:

**Branch (Sinatra)**
```
class Branch < ActiveRecord::Base
  belongs_to :user
end
```
**Branch (Ruby on Rails)**
```
class Branch < ApplicationRecord
  belongs_to :category
  belongs_to :city
  belongs_to :user

  has_many :guests, through: :attendees, class_name: 'User'
  has_many :attendees
  has_many :comments, through: :attendees

  validates :name, :organization, :date, :location, :info, presence: true
end
```

Where as prior, branch would simply belong_to: user, with Branch-v2 I set out to add a more common sense user experience. Particularly, cities and categories to which a branch could belong to, as well as validations built into the model to prevent null submissions. Being so that a branch would belong to a city and a category I set out to create nested structure (city/:id/category/:id/branch/:id) to support this.

In theory it seemed simple, a **City has_many: categories** and a **Category belongs_to: city**. Well perhaps that was too simple... Maybe I was stubborn, but I knew I wanted Categories that persisted across all cities (if nothing else, to make for an easier admin experience) and I was going to let nothing get in my way. While of course, I knew this was possible with a has_many :through association, I could not have anticipated how much trouble I would find myself having with forms and params passing between nested routes.
Routing Error
No route matches [PATCH] “/cities/4/categories/3”

> …while learning itself is a reflective act, it seems ironic to think how seldom we may find ourselves learning, well, ‘how to learn’.

The more I attempted to blindly fix the issue, the more I became convinced I was in over my head. While in past cases, I'd have flipped through my notes or revisited previous labs; my frustration lead me to believe I was dealing with something far beyond the scope of my knowledge. Hellbent on getting my models to play nice, I tried every minor variation "under the sun" (**read:** "on StackOverflow") over the course of a week. By the end, I was no closer to my goal and started to question if what I was attempting was even possible...

After a brief hiatus, I returned to my project and slowly began to realize the true problem. What I was attempting to do wasn’t wrong, but rather the questions I was asking were (and I asked a lot of them).
> I was manipulating forms and methods, hoping to create a new **category** under a single parent: **city**, while simultaneously creating it across multiple **cities**.
Even as I type that, I realize how little sense that makes in practice… I simply assumed it was more complex than I was capable of, and ignored my fundamental knowledge in favor of a easy answer. Reflecting upon what i had learned, I decided it best to allow for categories to be created and manipulated, independent of their city parent.

I would go from this:

**WHAT ARE THOSE!?!?**
```
Rails.application.routes.draw do
  devise_for :users
  root 'welcome#home'
  # resources :attendees
  resources :cities do
    resources :categories, only: [:index, :show, :new, :edit] do
      resources :branches, only: [:index, :show, :new, :edit]
    end
  end
end
```

To this:

**The Holy Grail**
```
Rails.application.routes.draw do
  devise_for :users, :controllers => { :omniauth_callbacks => "callbacks" }
  root 'welcome#home'
  # resources :attendees
  resources :cities do
    resources :categories, only: [:index, :show] do
      resources :branches
    end
  end

  resources :categories
end
```

With this in place, the rest of the application fell into place and I was able to fully realize my applications potential! As I look back on my Rails project, I’m glad I committed to my initial vision and feel I’ve certainly come to appreciate the true importance of allowing your mind to reset, and see things in a fresh light. After all, learning can only truly take place when one’s mind is open and receptive to new ideas — something that hours of aimless debugging can certainly hinder. Again, often times it’s not the answer, but the question that matters, for even if you can find a solution, if you’re unable to make the connection to what you already know… How do you learn?

> Thanks for Reading! Visit me on Github (http://github.com/SCDesigns) and play around with this project at (https://github.com/SCDesigns/branch-v2)
