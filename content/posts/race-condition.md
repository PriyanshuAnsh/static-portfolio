---
title: "What are Race Conditions?"
date: 2026-07-16
tags: ["Synchronization"]
---

# Explaination
When learning about Synchronization, `Race Condition` is a term that we engineers comes across a lot. So, what are race conditions? 

Look at the following code:
```
var data int = 10
go func () {
	data = 20
}()
fmt.printf("Data: %d", data)
```

The code is written in GoLang; (I'm learning GOLANG!!). Its a pretty easy Go Code,  the important piece of code is the keyword `go`. It creates a `goroutine` that execute the function concurrently; that set's data to 20. 
What you think we get as an output? 

> A. Data: 20
	
> B. Data 10

The Answer: We don't know. This is a race condition code. The code is fighting to change the value to 20 or keep the value to 10. 
The `go` keyword run the function that sets data concurrently. Henceforth, data = 20 is running parallel to the actual code. 

We will see option A, if the `goroutine` finishes before printing data to the standard output. 
We will see option B, if the `goroutine` finishes after printing data to the standard output. 

# TL;DR
>  Imagine, you and your sister fighting for TV remote control, the first one to get the remote wins. Similarly, the first to modify the "important code section"; (we call it a critical section), wins! 


# Note

Thank you for reading my messy article. I'm just starting to write blogs. In this world of AI, I think if I don't write something myself, I'll lose the ability to even write. 
