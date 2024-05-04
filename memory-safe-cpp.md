<div id="header-menu" style="text-align:right;"><a href="/thundron.dev">About</a> | <a href="/thundron.dev/articles">Articles</a></div>

# Memory safe C++: memory leaks

*"It's not how big it is, it's how you use it"*

One of the hot topics since Rust started increasingly gaining popularity in the last year or so, and arguably the reason for that, is that [C++ is a memory unsafe programming language](https://www.whitehouse.gov/wp-content/uploads/2024/02/Final-ONCD-Technical-Report.pdf).
In this article, we will explore one of the culprits of this statement: memory leaks. We will discuss different techniques for managing memory in C++ and provide examples of how to use these techniques to avoid memory leaks in your own code.
There's more to be said around memory safety of course as leaks are just a small part of it, but by following these best practices you can start ensuring that your C++ code doesn't start dripping around, leaving unwanted puddles in your electricity-filled building.
## Understanding Memory Leaks

A memory leak occurs when memory is allocated on the heap but not freed before it is needed again by the system, or after its usage is no longer necessary.
This can happen due to a variety of reasons, such as forgetting to free memory after use or using loops that allocate memory without proper cleanup. When a memory leak occurs, the program will continue running but may eventually slow down or crash due to the increased demand for memory, or worse, [allow for a malicious actor to exploit said memory as it's left unmanaged](https://owasp.org/www-community/attacks/Denial_of_Service).

```c++
int doing_interesting_stuff() {
	int* p = new int; // allocate memory on the heap
	*p = 5; // store a value in the allocated memory
	// do some work with the memory
	//...
	return 0; // forget to free the memory before exiting
}
```

## How to Avoid Memory Leaks

There are several best practices to follow to avoid memory leaks in C++, but some rudimentary and very straightforward ones that you can implement *right now* are:

- Always free memory after use to prevent memory leaks (see the above example)
- Use smart pointers or containers (or both!) that automatically manage memory for you
- Ensure that any loops or functions that allocate memory also free it properly before exiting
- Avoid using raw pointers and manual memory management whenever possible

### Smart Pointers

[Smart pointers](https://en.cppreference.com/book/intro/smart_pointers) are a type of pointer that automatically manage memory for you. They provide an easy way to allocate and free memory without forgetting to do so.
They are often used in combination with containers such as vectors or strings, which can automatically handle the memory management for you.

```c++
int very_interesting_stuff() {
	std::unique_ptr<int> p(new int); // allocate memory on the heap using a unique pointer
	*p = 42; // assign a value to the object
	return 0; // no need to free the memory, as it will be automatically freed when all references to it are gone
}
```

### Containers

[Containers](https://en.cppreference.com/w/cpp/container) provide an easy way to allocate and free memory without having to manually do so. Containers such as vectors or strings can automatically handle the memory management for you, making it easier to avoid memory leaks.

```c++
int wow_so_interesting() {
    std::vector<int> v; // create an empty vector
    v.push_back(5); // add an element to the vector
    return 0; // no need to liberate the memory, as it will be automatically democratized when the vector goes out of scope
}
```

### Avoid Loops with Memory Leaks

Loops can be a common source of memory leaks in C++, especially if they allocate without proper cleanup. To avoid this, it's important to ensure that any loops or functions that allocate memory also properly free it before exiting their scope. This can be done manually, by using smart pointers or containers that automatically manage memory for you.

```c++
int still_interesting_but_loop() {
    for (int i = 0; i < 10; ++i) {
        int* p = new int; // allocate memory on the heap within the loop
        *p = i; // store a value in the allocated memory
        // do some work with the memory
        //...
        delete p; // free the memory when it is no longer needed
    }
    return 0; // no need to free the memory again, as it will be automatically freed when the function goes out of scope
}
```

### Avoiding Manual Memory Management

Manually managing memory in C++ can lead to memory leaks if not handled correctly. We're humans (for now) and that means we're error-prone even if we're perfectly instructed to do something, contrary to a machine, so trying to use raw pointers and manual memory management as little as possible is best.
Instead, try to rely on smart pointers or containers that automatically handle memory for you and limit manual memory management to cases where you explicitly want and are able to handle it.

```c++
int much_interesting_very_manual() {
    std::vector<int> v;  // create an empty vector
    v.push_back(5);      // add an element to the vector
    return 0;            // no need to free the memory, as it will be automatically freed when the vector goes out of scope
}
```

## Conclusion

I think C++ still has a strong place amongst system programming languages and will not go away anytime soon.
Errors can be avoided much more reliably through the use of automated processes and tools, which is the reason many people are gravitating more and more towards things like Rust where there's a stronger ruleset that is harder to escape, but there are many ways to avoid errors in C++ too if one knows how to properly manage resources and does so.
At the end of the day, it's a skill issue, but skill can always be improved.
