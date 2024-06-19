# mtb-agi-rag-demo
Why not do a demo on something you are interested in!

| | |
|-|-|
| <img width="688" alt="image" src="https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/63755df1-c559-4e87-b26b-e7db8557c26c"> | Unfortunately I don't have it running live at the moment, but you can watch some videos and check out screenshots below. I did a live demo in Austin to a host of account executives and sales engineers. This was my "You win a car. And you win a car." moment. |


<br/><br/>
# Why do this?
I wanted to show off how easy it was to integrate Rockset query lambdas (SQL queries wrapped in API calls) with OpenAI for the purpose of building a RAG application.

Rather than show off a bunch of vectors, I decided to build a real app for it. I also decided to throw a front-end on it to make it more visual.

Here's what I used:
- vector database: Rockset
- text to vector embeddings: OpenAI
- kafka: Confluent Kafka
- large language models: OpenAI
- parallel processing: LangChain
- programming language: Python, shell scripts
- webapp: Flask, Jinja2 templating, bootstrap


<br/><br/>
# Videos
I created these clips to show our marketing team (at their request) more about the demo. They wanted to take these clips, watch and share them internally and then produce higher quality production videos. These were meant to serve as just a jumping off point for their own creativity, but I was going to be the start :).

We never got a chance to do it, but it would have been fun!<br/>
[1. Video Message Exploring Rockset with Cool Mountain](https://youtu.be/msWn6PqsULU)<br/>
[2. Cool Mountain Biking Next-Gen Trip Planner](https://youtu.be/e99vXPXdafQ)<br/>
[3. Data Flow from Kafka to Rockset](https://youtu.be/TaWbKlpotkQ)<br/>
[4. Similarity Search](https://youtu.be/h7dSqQSCJfw)<br/>
[5. Fun and Interactive Choose-Your-Own-Adventure Demo](https://youtu.be/2ewCWl2kT6Y)<br/>
[6. Trip Planner Option 1](https://youtu.be/XNzFU4pdmWg)<br/>
[7. Trip Planner Option 2](https://youtu.be/WPg-LJhAopk)<br/>
[6. What's next?](https://youtu.be/GVEYBhf0Lwc)


<br/><br/>
# Splash Screen
I am a representative of Cool Mountain Biking Incorporated. Or as Dun & Bradstreet list, doing business as Cool MTB Inc.

![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/397ad16a-6802-4254-a70e-30fab7b7c8a6)

<br/><br/>
# Landing Page
|          |          |          |
|----------|----------|----------|
| What we have    | ![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/ff2cea72-41b4-46b2-829a-ac144604de28)  | What are we looking at here? It’s the problem description. As a mountain biker, we have trails. Lots of trails. But what we don’t have is an awesome new age mountain bike trip planner that helps you figure out what to pack, what to buy, which trails to ride and how difficult they may or may not be based on your skill level.    |
| What we need    | ![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/a9ad90cc-a1ea-4035-b215-ce518c797510)  | That’s where Cool MTB, Inc. comes in. We are partnering with Rockset, OpenAI and Confluent to bring you the next gen MTB trip planner. OpenAI is bringing artificial general intelligence into the fold Rockset is bringing the real-time search and analytics platform into play, which incidentally will help AGI fill in blind spots. After all, AGI doesn’t know what it doesn’t know and I’d like to try to minimize hallucinations and blind spots. And Confluent will be bringing most of the data. Now to contain this demo a bit, we’re using mock generated biker and trail data. But imagine if Cool MTB, Inc. was to also partner with Strava for biker personalization and Trailforks or AllTrails for trail metadata. And what if it was to then partner with local retailers and online giants to allow personalized shopping lists and experiences. You get the idea.  |
| 50,000 feet    | ![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/b5209a5e-2dc8-4278-8ff4-b7ad67aba719)   | At a high-level, here’s how this is working. This should look familiar. I stole this from rockset.com. It’s “data in motion” from Confluent Kafka to Rockset.    |
| Ground level    | ![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/641a75d3-fe00-4ec2-91b3-a4b0caaab06b)  | Now, at a lower-level, here’s even more detail. This is a bit much to look at, but what I want to call out is we’re taking Biker, Trail and Leaderboard data from Kafka topics and moving that data into Rockset. On the way in, we’re also enhancing the trail data with text to vector translations using OpenAi’s API. For example, a trail description may say “The thrill of jumps that end in soft, forgiving landings” and that will be converted to an array of numbers and that array is saved in Rockset. Why is that interesting? It’s interesting because that’s going to help facilitate retrieval augmented generation. Said another way, it’s helping reduce blind spots using AGI by pairing up mountain biking trails from Rockset to recommend along with a nicely crafted unique mountain biking plan generated from OpenAI. Before moving on, let me say that what I want to show you is the MTB Trip Planner here on the left. That’s the demo. But to tie it all together, there are a couple of things I want to show you first. I am not going to walk you through any boring data setup, don’t worry, but I do want to touch on these bits, namely the Leaderboard and the Similarity Search.  |

<br/><br/>
# Leaderboard Page
Quickly, pun intended, I want to show you how data moves and that I’m not doing any magic tricks here. This is very simple.

![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/8e1a0840-74f3-4295-ad92-01ec0ffad49c)

|          |          |          |
|----------|----------|----------|
| Data flows from Kafka to Rockset    | ![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/fe234d5b-3963-4d9c-9b55-54dff21fa5eb)   | Data is flowing from Kafka to Rockset. I think most of you know, but just in case, Kafka is very simply an immutable log of data. It stores bytes. It doesn’t care what the data is. Just 0s and 1s effectively. But we need to make sense of those 0s and 1s and that’s where Rockset comes in. Our integration turns those 0s and 1s into usable JSON. And here is what a sample message would look like.    |
| Kafka Topic    | ![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/98563bdb-5fe3-4467-b41c-77a12daf030f)  | And here for instance is my Leaderboard Kafka topic configuration. As you can see, it’s not doing any fancy setup. It’s just a plain ol’ Kafka topic.    |
| Rockset Connector    | ![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/a3cd40fa-d026-4341-83d8-a6c1920da07d)  | Here is my Rockset native connector. We just specify the Kafka topics and the Rockset connector takes care of the rest. Some folks at this point may be asking, if I have all this data in Kafka already, why in the world do I want to move it to Rockset to do anything? Why not just do it in Kafka? |
| Rockset Collection    | ![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/a02c4bfa-1942-4fe5-a32f-48383a212933)   |  Well, this is why. Here is a very simple query. I peppered in some stuff to make a point. I’m doing a common table expression with some row number ordering by partition along with some joining and grouping. I’m not doing that in Kafka. I might be able to do some level of aggregations and windowing functions in Kafka. Maybe some KStream metadata materialized into KTables, but still. I’m not and probably can’t do all this in Kafka no matter how much I butcher it. And that’s not what Kafka was designed and built for.    |
| Live Leaderboard | ![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/2649e4c0-d81e-442f-a593-62c8212911b6) | This leaderboard is fetched in real-time with sub-second latency |

<br/><br/>
# Similarity Search Page
Now the last thing I wanted to show you is equally important. This is one way of how natural language text is turned into a usable set of vectors in Rockset.

![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/2f989414-ea07-43fb-9b3c-82aca57ce83f)

|          |          |          |
|----------|----------|----------|
| Text to vectors    | ![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/50fc0066-cfac-4de7-8db8-4acbae4daf04)  | Whatever we type in the input box above, we are converting that into a series of numbers using OpenAI. And then we’re using that series of numbers to query Rockset. That works because, if you recall, the trail data from Kafka was accentuated with vector data from OpenAI. And you can see below, the FROM _input gives it away, that this is a SQL ingest transformation for source data. And we’re using the VECTOR_ENFORCE function to tell Rockset this is special data, treat it kindly please.    |
| COSINE_SIM() similarity    | ![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/871123bb-3635-470d-b1df-5af412078223)  | Now you can see the list of trails is returned to us using natural language conversion    |

<br/><br/>
# MTB Trip Planner Page
What you see here is a “choose your own adventure” demo. I think the one on the right is a little bit more interesting, but both are instructive. So we’ll do the left option first as if we were an actual app user.

On the left, AI will walk you through a multi-step process where you have control over your mountain bike trip planner. On the right, AI will just do everything for you in one fell swoop. On the right, AI will not only create a plan for you, but it will also recommend trails to ride that fit that plan. On the right, is the “I don’t want to to make any decisions, just do it for me” method. 

![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/6625ff5f-6b1f-40e6-9f37-5e0971ee6a03)

<br/><br/>
# MTB Trip Planner Option 1
In this diagram, I am wanting to convey what we’re doing here. 

In Step 1, we’re asking AI to build us a mountain bike trip plan, as provided in the input box above. In Step 2, we’re asking Rockset to get us a list of trails that match our plan. And in Step 3, we’re asking AI to take the original plan from Step 1 plus what we gathered from Rockset in Step 2 to combine our specific trails into a revised plan.

![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/60c7af2d-8813-4241-b226-6c9c0a73adcc)

You can choose a list of LLMs to choose from, select a number of "max tokens" which essentially limits the verbosity of your AI agent's response and even tell AI to hurry up. We do that in this case and AI very quickly comes back with a brief plan. Had we slowed it down and told it to be "thoughtful" and take its time, it would have produced a much larger, more comprehensive plan.

|          |          |
|----------|----------|
| ![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/e0f342b6-c0f2-4864-bb0a-5357753d8c4c)    | AI produces a very simple plan because we chose a fast method    |
| ![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/8cd9e803-c22b-4fde-a50e-fa8745d133a3)   | AI asks if you want to accentuate this plan with some real actual MTB trails    |
| ![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/a475b312-6f8e-4913-9e49-91b745265186)  | AI keeps a running chatbot history for your convenience    |

Now we proceed to Step 2. This is where AI has blind spots. AI is perfectly suited for creating a general plan just like this. But AI doesn’t have detailed up to date information on trails like a 3rd party does. It may even hallucinate to help fill in some of those blind spots. As a user though, this may be sufficient. You can either use this plan as-is or fill in those blind spots using actual trail data. Let’s do that.

![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/fd1b1b6c-f829-48ed-b1eb-070c56934052)

|          |          |
|----------|----------|
|  ![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/06c39bde-8558-4777-aed3-3fc35024150d)  ![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/a87a308b-dcbd-4d2b-8e24-d74415345f74)  | And voila, we get a much more comprehensive plan. Notice these are the actual trails we asked AI to incorporate into our plan! What happened there was behind the scenes, your phrase was converted into vectors and searched against Rockset. The top 5 closest matching trails to that criteria were retrieved. After all, you can’t ride 100 trails in a single weekend.    |


<br/><br/>
# MTB Trip Planner Option 2
So this one is a bit different. Recall from earlier, it’s not a workflow from a user perspective. This option is a “I don’t want to do any extra steps, you’re AI, you’re supposed to be smart, go to town and hook me up. And AI, please give me trails to ride, use your best judgment”

![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/125bf90b-46e9-4d1f-8ec8-079d3f716582)

|          |          |
|----------|----------|
| ![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/6b0886bd-3b9b-4b3d-a471-c452a2c31fd2)  | In this case, I also wanted to also show how Rockset can work with LangChain. LangChain makes it easy to create a process or pipeline of AI-type work. On the right, you can see pseudo-code. If I were a Rockset engineer, this would have taken me 10 minutes to figure out, but instead it took me hours. Anyway, the point is you can create parallel tasks that source data from multiple inputs very very easily with LangChain. In this case, I am using Rockset as a parallel, but I could have many, many more.   |
|  ![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/1fac230e-cb8b-44dd-a1c3-408777604128) ![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/e50af2e7-728f-4efb-bfd3-c12c63a6f01a) | And there you go, one fell swoop, AI worked its magic and came back with a plan incorporating trails in a RAG implementation. We could have asked Ai to respond in another language. We could go global with this planning app. AI will do it for you.  |


<br/><br/>
# What's next? Page
Alright, demo is done. Now all that is left is building out more and more and more.

![image](https://github.com/scottsappen/mtb-agi-rag-demo/assets/2436969/74f905ff-7d22-4f61-a54b-947ef4419ac5)





