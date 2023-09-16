---
author : "Sumit"
title : "Context in Go - Explained"
date : "2023-02-05"
description : ""
tags : [
    "go",
]
summary: "What, Why and How of Context in Go "
published : true
---

I have been using Go for a while now and I really enjoy its simplicity and concurrency features. However, one thing that puzzled me was the context package. What is it for? How does it work? And why do I need it?

In this blog post, I will try to answer these questions and share my experience with using context in Go. I hope you find it useful and informative.

## What is context?

According to the official documentation, context is a package that "provides functionality for passing request-scoped values, cancelation signals, and deadlines across API boundaries to all the goroutines involved in handling a request". In other words, context is a way of passing information and control signals along the call stack of your program.

## Why do we need context?

One of the main reasons we need context is to handle cancelation. Imagine having a web server that handles incoming requests from clients. Each request spawns a new goroutine that performs some work, such as querying a database, calling an external API, or doing some computation. Now, what if the client decides to cancel the request or the request has timed out ? How do we notify the goroutine that it should stop working and free up the resources?

This is where context comes in handy. We can create a context with a cancel function and pass it to the goroutine. The cancel function can be called by the calling function when it no longer needs the goroutine to be working on the request. The goroutine can then check the context for cancelation signals and abort its work gracefully.

Let's take an example to demonstrate how context is used along with the ```net/http``` package. We will make an http request to ```https://www.github.com``` and add a deadline for our request to timeout.

``` go
func main() {
    
  // Create a context with deadline   
  ctx,_:=context.WithDeadline(context.Background(),time.Now().Add(10*time.Millisecond))
  
  var buf bufio.Reader
  var url string = "https://www.github.com"
  
  // Create a new request and pass the context with deadline
  request,err:=http.NewRequestWithContext(ctx,http.MethodGet,url,&buf)
  if err!=nil{
      fmt.Println(err)
      return
  }
  
  // Make an http request request on url
  _,err=http.DefaultClient.Do(request)
  // Check the err, if the deadline has exceeded or not
  if err!=nil{
      fmt.Println(err)
      return
  }
}
```

In the above code we created a context with deadline by calling the context.WithDeadline function that takes a parent context and a deadline for the context to timeout. context.Background returns an empty context which we have used as parent and set a deadline of 10 ms. We perform a get request to ```https://www.github.com``` and check for any errors. The request takes more than 10 ms to complete, so the request times out and is terminated before completion.

Another reason we need context is to pass request-scoped values. Sometimes, we may want to associate some information with a request and make it available to all the functions involved in handling it. For example, we may want to pass a user ID, a trace ID, or some other metadata. Context allows us to store these values in a map-like structure and access them anywhere in the call stack.

## How does context work?

The Context package in itself is not very large. The package defines only three types and the entire source code is barely few hundred lines.
Context is an interface that has four methods -  

- ```Deadline()(time.Time, bool)``` - Deadline returns the time when the context will be canceled, if any.
- ```Done() <- chan struct{}``` - Done returns a channel that is closed when the context is canceled or expired.
- ```Err() error``` - Err returns the error that caused the context to be canceled, if any.
- ```Value(any) any``` - Value returns the value associated with a key in the context, if any.

We do not need to implement the interface ourselves, functions like context.Background() or context.TODO() return an empty struct that implements the context interface 

A CancelFunc is a function that cancels the context.

The context package also provides some functions to create and manipulate contexts:

- Background returns an empty context that is never canceled and has no values. It is typically used as the root of a context tree.
- TODO returns an empty context that is never canceled and has no values. It is typically used as a placeholder when you are not sure what context to use or if you are planning to add one later.
- WithCancel returns a copy of a parent context with a new cancel function. The returned context is canceled when the cancel function is called or when the parent context is canceled, whichever happens first.
- WithDeadline returns a copy of a parent context with a deadline. The returned context is canceled when the deadline is reached or when the parent context is canceled, whichever happens first.
- WithTimeout returns a copy of a parent context with a timeout. The returned context is canceled when the timeout expires or when the parent context is canceled, whichever happens first.
- WithValue returns a copy of a parent context with a new key-value pair. The returned context can be used to pass request-scoped values along the call stack.

In the first example, we used ```context.WithDeadline``` to create a context with a deadline. This example will demonstrate how to pass values with context and use ```context.WithTimeout``` for cancellation.

``` go
package main

import (
	"context"
	"fmt"
	"net/http"
	"time"
)

func main() {
	// Create a basic HTTP server
	http.HandleFunc("/", handler)
	http.ListenAndServe(":8080", nil)
}

func handler(w http.ResponseWriter, r *http.Request) {
	// Create a new context with a timeout of 5 seconds
	ctx, cancel := context.WithTimeout(r.Context(), 5*time.Second)

	//Set value in the context with a key
	// Note: Not a good practice to use in-built type as key
	ctx = context.WithValue(ctx, "myKey", "myValue")
	defer cancel()

	// Pass the context to another function that does some work
	result, err := doWork(ctx)
	if err != nil {
		// Handle error
		fmt.Println(err)
		w.WriteHeader(http.StatusInternalServerError)
		return
	}

	// Write response
	w.Write(result)
}

func doWork(ctx context.Context) ([]byte, error) {
	// Simulate some work that takes 10 seconds

	// Get the value set in the context 
	fmt.Println(ctx.Value("myKey"))
	time.Sleep(10 * time.Second)

	// Check if the context is canceled
	select {
	case <-ctx.Done():
		// Return an error
		return nil, ctx.Err()
	default:
		// Return some result
		return []byte("Hello, world!"), nil
	}
}

```

In this example, we create a new context with a timeout of 5 seconds, set a value and pass it to the doWork function. The doWork function simulates some work that takes 10 seconds and checks if the context is canceled before returning. If the context is canceled, it returns an error. Otherwise, it returns some result.

If we run this program and send a request to the server, we will see that the request is canceled after 5 seconds and the server returns an internal server error. This is because the context is canceled by the timeout and the doWork function returns an error.

However, if we send a request and close the browser before 5 seconds, we will see that the request is also canceled and the server prints "context canceled". This is because the context is canceled by the web server when it detects that the client has disconnected.

### Conclusion

In this blog post, I have explained what context is, why we need it, and how it works in Go. I hope you have learned something new and useful. Context is a powerful tool that can help you handle cancelation and pass request-scoped values in your Go programs. However, it also comes with some caveats and best practices that you should be aware of. For more information, I recommend reading the official documentation and this [blog post by Dave Cheney](https://dave.cheney.net/2017/01/26/context-is-for-cancelation).

Thank you for reading and happy coding!
