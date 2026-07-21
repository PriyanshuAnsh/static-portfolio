---
title: "What are Channels in Go?"
date: 2026-07-21
tags: ["Synchronization"]
---

# What are Channels in Go?
Channel are Concurrency Primitives in Go. The concept of `channel` was inspired from research paper `Communicating Sequential Processes` by Tony Hoare in 1985.  
The philosophy of `channel` is as follows:

> ***Don't communicate by sharing memory, but share memory by communicating.*** 

Concurrent programming can be divided into two parts.
1. Communication by sharing memory:
	For a concurrent process, you pass the address to the resource that is modified; along with a mutex. 
	
	```
	func main() {
		mu := &sync.Mutex{}
		wg := &sync.WaitGroup{}
		resource := 10
		wg.Add(1)
		go func() {
			defer wg.Done()
			mu.Lock()
			resource += 15
			mu.Unlock()
		}()
		
		mu.Lock()
		resource *= 15
		mu.Unlock()
		wg.Wait()
		fmt.Println(resource)
	}
	```
	Execution of the above code block could result in:
	>  A. 165
	
	>  B. 375
	
	Although we have eliminated race condition, but depending on which go-routine runs first, `resource` is undeterministic. 
	
2. Share memory by communication: 
	For a concurrent process, you can create a pipe between the two processes/Goroutines. The GoRoutines then can fetch values from the pipe, modify it and put it back in the pipe. In Go, this `pipe` is nothing but the `channel`.
	
	```
	func main() {
		channel := make(chan int, 1) // This is called a Buffered Channel
		wg := &sync.WaitGroup{}
		resource := 10
		channel <- resource // We seed the channel. This is called Write to the channel. You can think of it as resource is going into the channel as 'resource' is right and arrow head is to channel.
		wg.Add(1)
		go func() {
			defer wg.Done()
			val := <-channel // This is called Read from the channel
			val += 15
			channel <- val // Put value back in channel
		}
		val := <- channel
		val *= 15
		channel <- val
		
		wg.Wait()
		resource <- channel
		fmt.Println(resource)
	```
	
	Executon of this will always deterministically result in resource = 165.


That is the essence of the popular saying. 

The read and write from the channel are blocking; meaning, if the channel is empty, it will always block the read or write go-routine. Henceforth, it's an important step in the above example to seed the channel with the resource value. If you don't seed the channel and execute the program, you'll encounter a deadlock and the go-compiler will get angry at you!
	
	
	
