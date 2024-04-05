+++
title = "Introducing Tilecraft: A Powerful and Flexible Tile-Based Map Generation Tool"
date = 2024-04-03T02:56:56-06:00
draft = false
tags = ["Rust", "development"]
series = ["Tilecraft Build Log"]
+++
## Introduction
Last year, when GPT-4 was released, I, like everyone else, was captivated by its impressive capabilities. A year later I still use it every day and wouldn't want to go back to a time before it. But it didn't take long after first using it to start probing the depths of its limitations. My girlfriend and I had started playing around with having it serve as a dungeon master for whatever crazy scenario we could think of to throw at it and it did its best but, and I'm sure you can already think of several of them, there are a whole host of reasons why ChatGPT falls horrendously short of an effective dungeon master. For one, it yearns to please. A quest gets pretty damn monotonous in a hurry when the DM just goes along with everything you say and never bothers to run stat checks or perform dice rolls of any kind to determine the success or failure of any given outcome. I experimented with creating a custom GPT with a pre-prompt instructing it to execute Python code to roll a digital dice to determine the success of actions but it seemed either unwilling or just plain couldn't remember to do so during the course of a quest. Even when I asked it explicitly to use random number generation to determine the outcome of an action, the result always somehow seemed to come out in favor of the players. 

But with the launch of GPTs, OpenAI also released the ability extend a GPT's capabilities by telling it how to access APIs. This is when a light bulb turned on for me. Perhaps I could build an API, or set of APIs, that could enable a sufficiently advanced LLM to query and update all the game parameters it needed to serve as an effective DM. The APIs could handle all of the stuff for which a language model is woefully ill-suited, i.e. managing game parameters and logic, leaving the LLM to focus on what it's actually good at, ingesting and regurgitating data in a highly digestible, coherent, and engaging way.

## So WTF is Tilecraft already?
After mulling the idea over in the back of my mind for a few months, I realized that there were several intertwined pieces to this puzzle. To do what I had vaguely conceptualized, there would need to be systems for creating and managing characters, for handling goals, actions, and outcomes for quests, for running game logic and updating world state, and more. One of the biggest challenges I could foresee was in building **a system for generating, editing, and querying game maps**. My vision had become to make each of the game management systems flexible enough to accomodate any kind of role-playing adventure imaginable. For the character management system, that would mean customizable character sheets where, if your game world doesn't really care about stats like dexterity, stealth, or strength but stats like flexibility, fashion sense, or fluid retention might be relevant, then you could use those instead. 

So in the case of maps, that meant allowing fully customizable metadata to be stored in each tile was going to be the goal. In addition, the whole thing about maps is that it's not just about what's on a tile but also, naturally, that the tile's data makes sense in relation to that of the tiles around it. That, in a nutshell, states the goal of the project that I'm currently calling Tilecraft; to create an insanely flexible tile-based map generation tool.

## The Nitty-Gritty
I decided to build Tilecraft in Rust. I landed on Rust for a few reasons, some personal and others practical. I come from a mostly Node.js development background and wanted to seize on the opportunity to learn a new backend language. On the practical side, I knew the app would have to be highly performant as layer generation algorithms can be __ridiculously__ compute intensive. I'd heard the name Rust occasionally and had absorbed that ot was basically "the new C++" so I figured I should see what all the fuss was about. And, after a month now of working on the project I only continue to believe it was the perfect choice. Rust and the Rust ecosystem have brought systems-level development into the modern era with its own Swiss Army knife of a package manager Cargo which can also build and run your project and even includes a built-in unit test runner so others like me coming from a Node/NPM environment can slot easily into that paradigm. The language includes a highly sleek and modern type inference system as well, similar to that of Typescript, making the manual type annotation of legacy systems-level languages like C/C++ look even more archaic. 

In choosing which existing frameworks to use for the project, I landed on two of the most highly-recommended and established names in the world of Rust; Actix-Web for building my server and Diesel as my ORM.

## State of the Project
Currently, I'm at the point in the project where all of the "grunt work", implementing constructor modules for my Actix app and establishing Diesel database connections, creating the database models and migrations, creating services for database CRUD operations and file storage, and defining endpoint controllers and routes for each resource, is just about complete. However, the heart of the whole thing, the layer generation algorithm(s), is where the next big chunk of the work lies. Once I have all of my API routes in working condition, I plan to implement a simple front end map visualizer system so I can actually assess what the output of my generation algorithm looks like. Then from there it's off to the races. I can start tweaking the layer generators to my heart's content and eventually, hopefully, get it into a condition where I can start inviting a limited audience to try it out for themselves. I also have big plans for a Map Studio front end app that allows users to generate, view, and manually edit their maps down the road and plan to develop that as a fully open-source once project once the back end is in a sufficiently satisfactory state.

## Coming Up in the Series
In this series, I plan to walk through some of the challenges and triumphs I encounter (or already have encountered) along the path of building this crazy thing. In the next few posts I'll go over some of the journey I've been on thus far, from taking my first baby steps in learning Rust, to some more technical deep-dives on topics like setting up Diesel and PostgreSQL, designing my tables and models, setting up unit and integration testing, and much, much more. I'll also get into some of the more high-level system design decisions I've made and other aspects of my vision for this thing. I do plan to keep the project closed-source as I don't want to give away too much of what may become a valuable "secret sauce" in the future but I am open to collaboration should the right devs be interested. Don't hesitate to reach out with any questions, feedback, suggestions, or just to chat! I read all the comments and can also be reached by email at david@david-white.dev.