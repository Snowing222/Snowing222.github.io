---
layout: post
title:      "PManager-rails-app "
date:       2020-11-10 19:35:15 +0000
permalink:  pmanager-rails-app
---

Pmanager is an app made for production managers in fashion production. It aims to help product managers to manage their products' life cycle more efficiently.

It took me a good few days to decide the topic of my rails project. I chose it because I used to work in the industry, and I know the pain points as product managers. The product manager handles tons of products, and they are the liaison between the company and outsource factories. They have to communicate with different departments within the company, and with different factories, to make sure all products are delivered on time.  Meanwhile,  all products are in different stages, require different actions, and have to meet the delivery deadline. Unavoidably something would fell through the crack along the way.  My apps will help them to avoid this kind of situation as much as possible.

After decided the topic. It took quite some time to set up the models that both make sense for my app and meet project requirements. I have to say project planning is one of the most difficult phases in my project. At my current phase in the curriculum, I'm constantly struggling between my vision for this app, what I can do, what I'm able to do. what I'm probably able to do with some stretch. I want to challenge myself, but not overstretch, and spent too much time on it. A big hurdle for me right now might be easily resolved with future lessons. I set up my models as below:

![](https://i.imgur.com/gH67uxe.jpg)

There are two types of associations I need to figure out. One is more static in my opinion and the other one has more motion to it. It's about the workflow, and it depends on the life cycle of each product and each sample.

a. Static part of the association:
A product belongs to a production partner
A product belongs to a manufacturing partner
A product belongs to a design partner
production partner, manufacturing partner, design partner are all user classes. I will have to set up the association using class_name and foreign key.
A product belongs to a factory. A production partner manager many factories through products.

b.workflow of each cycle

A product has to go through different sample phases in order: Proto, Fit Sample, PreProduction Sample, Top of production. It's only complete once TOP(Top of production) is approved.

Each sample has to go through different stages in their life cycle.  Status class represent each stage, and there is a current_state, and owner_id(the partner who in charge of the current_state)

Stage 1: The production partner requests a new sample from the manufacturing partner. The responsible party moves to manufacture partner, status is updated to pending sample from manufacture partner.

Stage 2: The manufacturing partner made and send the sample back to the production partner. The product manager received the sample and pass the sample to the design partner to review. The status of the sample is updated to the pending review from the design partner. the responsible party is updated to the design partner.

Stage 3: Once the design partner finished the review, they will either approve or reject the sample, and the production partner will
ask manufacture partner to resubmit, or move on to the next sample, send next level sample request.

When I create a product if the product does not have any active sample. I present the option of requesting a new sample.
 
When I click requesting a sample. There are one sample creation and initial status creation fired at the same time. Once both instances are created successfully. There is a next action button presented to the user base on the sample current state.

This is a very industry-specific domain. I tried to design it to reflect the real world as much as possible.

Looking back, there are some of my challenges:
1. When it's come to a model property. When should I use Enum, and when it requires a real model?
2. When one model's attribute depends on/ has a certain relationship with another class's attributes. Does it deserve its column? 
3. What is the workflow to handle a project from beginning to end.
4. How to present different action base on different sample status.
5. How to improve user experience.
6. Refactoring the code to make it's easier to maintain.

It's an "odd" domain, and I learned a lot and realized there is still so much to learn.  Sometimes, I feel overwhelmed and discouraged, but I keep reminding myself. Start from solving the smallest problem and one step at a time.
