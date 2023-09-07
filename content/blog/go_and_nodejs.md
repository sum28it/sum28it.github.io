---
author : "Sumit"
title : "My learning experience with Go and Nodejs"
date : "2023-06-14"
description : "Lessons learned from my experiences"
tags : [
    "go",
    "nodejs",
    "experiences"
]
summary: "A little talk about my expeprience with Go and Nodejs."
published : true
---

## My intro to backend development

A year and a half back, I started learning web development or to say server-side development in particular. Some of the most popular options I found were Nodejs, Java and Python. Even among those, Nodejs seemed to be everywhere you look. It has one of the largest communities and resources to learn and get help from. As someone new to backend dev, most places suggested to start with nodejs or python, may be django or flask. I already had some experience with Javascript and React so I decided to go with Nodejs.

## My Roller-Coaster ride with Nodejs

![nodejs roller coaster ride](/images/blogs/go_and_nodejs/roller-coaster.svg)

The first thing that I found on Youtube was like ```build a web server with nodejs and express```. While it was super easy to build a server with just a few lines of code, I didn't understand a bit how it's working, I could just see it working. And the fact that nobody was teaching nodejs, everybody just seemed to build web server, create a few endpoints to build a CRUD app and that's it. It was a bit annoying, like I see it working, I can build it on my own machine as well, but I don't understand what's going on behind the scenes.

I decided to take a break from videos and read the documentation. I headed to [nodejs.dev](https://nodejs.dev/en/learn/) to learn the fundamentals. I benefittd a lot from this section and started to read more. I searched for books but surprisingly found very few good books. This was unexpected for such a popular technology. After some more effort, I finally decided to build a project. It wasn't a very complex one, but I tried to put in test what I had learned so far and build a RESt API with CRUD features.

## How I got introduced to Go ?

Choosing to learn Go wasn't due to any technical reasons or flaws of Nodejs. I dare not say that I had run into some performance bottleneck that made me switch to go. Infact, most web or application servers do not have that high performance requirements and generally bottlenecks are present somewhere else in the system. 

After building some simple REST based apps, I decided to learn about software architectures. One very buzzing word was Microservices. It can be heard all over the web how it's the present and the future of software architecture. Whether it is or not, is a topic  for some other day. Having the ability to build polyglot softwares was one of the key benefits of microservice architecture. Go was one popular choices for building microservices, some of the reasons being it's high performance, concurrency and scalability.

## Getting started with GO

![go-mascot](/images/blogs/go_and_nodejs/go-mascot.svg)

Before Go, I had a good bit of experience in C, C++, so picking up a new language like Go was not very difficult. The syntax highly resmebles to C and a little bit to python too. I looked for some courses and found a good one on [Coursera](https://www.coursera.org/specializations/google-golang). I joined the Go community on reddit to see how people use Go and recommend learning it. The [Go tour](https://go.dev/tour/) was one must do thing recommended and it was a pleasant learning experience as well. The Go community was very helpful and resourceful as well. Although there was some bias, but it's pretty normal and can be found with anyone working with any particular technology for long time.

Having known the syntax and basics of the language, I decided to build a project to solidify my understanding and built some CLI tools and REST APIs. One very likeable aspect of Go was it's standard library, and I'm not the only one saying that. The standard library covers most of the basic building blocks required, and is very well documented too. It was the first time I had read any documentation that thoroughly and happily.

## Why I sticked to GO ?

For the first three years in my college I learned and played with many different technologies. Different languages, types of databases, software architectures, deployment techniques, and a bit of Machine learning as well. So, in the final year, I decided to streamline myself and focus on backend development and distributed systems. 

I went with Go, as till then I was comfortable enough to read the documentation and even browse through the source code if needed. Being able to read the soure code was such a confidence booster. I read more books to learn about best practices and idiomatic ways of writing. 

## Which side am I leaning now ?

There must be very little doubt about this question after reading through here. I am definitely more inclined towards Go, but I also do plan to learn Nodejs again and in depth. The easy going with Go can be credited to my previous exprience with Nodejs too. Before starting with Go, I was already well versed  









<!-- 
``` javascript
// Importing express module
const express = require("express")
const app = express()

// Handling GET / request
app.use("/", (req, res, next) => {
	res.send("This is the express server")
})

// Handling GET /hello request
app.get("/hello", (req, res, next) => {
	res.send("This is the hello response");
})

// Server setup
app.listen(3000, () => {
	console.log("Server is Running")
})

```


```go
package main

import(
    "http"
    "log"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request){
        w.Write([]byte("Go server"))
    })
	http.HandleFunc("/hello", gefunc(w http.ResponseWriter, r *http.Request){
        w.Write([]byte("Hello Response"))
    })

	log.Fatal(http.ListenAndServe(":3333", nil))
}
```










 -->
