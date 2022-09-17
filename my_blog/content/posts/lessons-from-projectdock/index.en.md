---
title: "Learning From Experience: ProjectDock"
date: 2022-09-11
draft: false
tags: [
    "Personal Projects"
]
resources:
- name: "featured-image-preview"
  src: "banner.png"
---

Oxford Languages defines the term retrospective to mean "looking back on or dealing with past events or situations" but what does that really
look like in practice? How does one effectively "deal with past events or situations"?

Coming from a software development perspective with a background in Agile methodologies my mind immediately jumps to the notorious Sprint Retrospective.
Although different teams may conduct their retrospectives in a variety ways, in my experience there have been three key components of the retrospective
process that contribute directly to improvement in the long run.

### 1. What worked well?

A sprint is a way of defining a period of time during which your team will set out to deliver a fixed amount of work. If you don't have any record of a
previous sprint, this is the first step to a successful retrospective: Taking inventory of what went well, and thus what the team should continue doing.

In my case, one of the biggest challenges with ProjectDock was that it was a project built only by 1 person: me. Despite the many challenges that arose
due to the lack of manpower, it's still important to touch on the positives :)

1. Efficient Feature Development
    - A reason you might consider starting a larger full stack application on your own is that you have free reign over the development. In my case, 
    this was definitely a strength as the development speed was relatively quick.
    - The entire project from start to finish took roughly 2 months including 2 major setbacks that required me to rebuild the project from scratch twice.
    But we'll touch on that further down.

2. Accelerated Learning Opporunities
    - Another positive of tackling the project alone was that I had to do everything myself. From flushing out an idea, designing how the
    project would look, figuring out an acceptable architecture, deciding on a tech stack, performing the 
    QA duties and testing use-cases, purchasing a domain, deploying/hosting the application, managing the database
    rules, the list goes on and on.
    - Although the weight of the workload was heavy at times, it resulted in equally significant learning outcomes.

### 2. What can we do better?

The next, and in my opinion most crucial, step is to define what we can do better. This can include any variant of "What did we do poorly?"
or "What should we stop doing?", but the gist remains the same: What did we not do to the best of our abilities?

With ProjectDock, there were many points at which I could have changed the workflow to both improve the quality of the product and reduce the 
strain on myself as the sole developer.

1. ProjectDock Was Rebuilt 3 Times
    - The current version of ProjectDock was built using TypeScript and Angular, paired with Firebase (A Google product that offers an auth
    solution, free NoSQL datastores with high availability, etc). However, it started very very differently.
    - ProjectDock v1.0 kept Angular as the web framework, but used a containerized Django PostgreSQL setup as the server. Specifically, I was
    running Django in a Docker container serving as my REST API, PostgreSQL as my database, the Angular app that interfaces with my API also
    running in a container, NGINX as my production web server (Django's built in web server shouldn't really be used in production), and the 
    whole thing deployed to an AWS EC2 instance using Docker Compose. Yeah... bit of a mouthful.
    - Writing the server side code was fantastic as I had the opportunity to learn some nuances with the Django ORM, and building out the
    deployment workflow was pretty fun. The server itself was also fairly performant. But soon I realized that for ProjectDock, 
    this was way too overkill. I was cutting a cake with a sword when I really needed a kitchen knife. This amount of engineering would be 
    great if we were a team of developers, each with different machines and environments. But for a sole developer? Maybe not as much.
    - And so I researched some lighter-weight technology that would get me to my MVP quickly, settled on Firebase, and rebuilt the app from
    the ground up.
    - Once I got through some boilerplate code, I was progressing pretty quickly. A lot of the major screens were implemented, all of the service
    logic interacting with Firebase was performing well, and I had a good looking, functioning application.
    - However, this is when I made yet another mistake. Even though the app could be finished within a week, I decided to redesign everything
    to follow a different design system. I had a new, more "minimal", UI design for ProjectDock and embarked on the tedious task of rebuilding
    all of the viewable components.

2. Lack of Design Mockups
    - A significant crux for ProjectDock was the lack of a formal UI/UX designer. Working as a software engineer on a team, often you'll
    have a designer that provides beautiful mockups that you can use as a reference when building the app. This offers some stability throughout
    the build process, comfort knowing that core components won't be totally redone, and confidence knowing that a fellow
    professional has designed things in a way that is sure to be user friendly.
    - I can humbly admit that UI/UX design is far more difficult than it appears, and that it can be considered my most "felt" weak spot.
    - A lack of mockups leads to a development process that is unstable due to the constant rebuilding of design elements.

3. Scope Creep
    - Finally, scope creep. Throughout the project I often found myself adding more and more features. "Oh this would be great to add in".
    "This should be included in the MVP". These little additions here and there began adding up until the scope of the project was far larger
    than initially intended.
    - Scope creep is difficult to handle, but it can be deterred with a strong Definition of Done, some discipline in determining which features
    are "core" to the app, and adding a backlog to the project so you can visualize how many unintended ideas you're adding midway.

### 3. Defining Action Items

One of the final elements of a successful retrospective involve setting some action items. What actions are we going to take to work on the
areas of improvement and learn from our mistakes? Mine are as follows:

- Once a project's design is finalized, it's well... final.
- Take atleast 3 days when deciding which tech stack best suits the project. Because once you decide, it's extremely difficult to go back.

### Conclusion

In conclusion, building personal projects is an amazing experience that any and every developer should take part in. Sometimes it will get difficult.
Motivation is often temporary and you need systems and routine in place to stay on track with development. It's helpful to pick a project that 
you'll be passionate about, but it's also worthwhile to build something for the sake of learning a new tech or to keep some old skills in shape.

We all have projects that we've started and left unfinished. And that's okay. But the experience of bringing a challenging project over the finish line
is one that we should all strive to have atleast once. On to the next :)