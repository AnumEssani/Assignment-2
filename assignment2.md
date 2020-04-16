# Summary On Article Of Async

Hot off the heels of RustFest Barcelona and the stabilization of async/. Await. We’ll be creating a super simple application that fetches some data from the internet using the our newfound async abilities.The stabilization of async/.await also coincides with another case with recent entries in the mainline pokeman games.

# What does asyncmean?
       So asynchronous programming lets us run multiple of these IO-bound computations at the same time on a single thread. Multi-threading is ideal for when you’ve got computationally intensive tasks (so-called CPU-bound tasks) that can be spread across multiple, separated cores. They can run at the same time because when they’re waiting for a response, they’re just idle, so we can let the computer keep working on something that isn’t waiting. Concurrent programming is better suited for when the task spends a lot of time waiting, such as for a response from a server. In Rust, values that are ‘awaitable’ are known as ‘futures’.

# Rusty weirdness:
Async in Rust can be a bit different from what you used to do in other languages.  Here are a few main points to understand:

# An async function does not (necessarily) start executing immediately:

When you want to start an asynchronous feature, you must either .await it or  start a task using an executor (we'll get to that in a moment). Before that  happen all you've got is a future that hasn't begun:

```
Use async_std::task;
// ^ we need this for task spawning

Async fn negate_async(n: i32) -> i32 {
    Println!("Negating {}", n);
    Task::sleep(std::time::duration::from_secs(5)).await;
    Println!("Finished Sleeping for {}!", N);
    N * -1
}

Async fn f() -> i32 {
    Let neg = negate_async(1);
    // ... nothing happens yet
    Let neg_task = task::spawn(negate_async(2));
    // ^ this task /is/ started
    Task::sleep(std::time::duration::from_secs(1)).await;
    // we sleep for effect.

    Neg.await + neg_task.await
    // ^ this starts the first task `neg`
    // and waits for both tasks to finish
}

```

So in the above little code snippet, here’s what’s going on.
    1. The async function f is more interesting: The first line (let neg ...) creates a Future of the negate_async function and assigns it to the neg variable. 
    2. The async function negate_async takes as input a signed integer, sleeps for 5 seconds, and returns the negated version of that integer. 
      
So what’s the result of this, then?

```
 Negating 2’’
# <- there's a 1 second pause here
Negating 1
Finished sleeping for 2!
Finished sleeping for 1!

```

As we can see, the second future, neg_task, started executing as soon as it was called—thanks to task::spawn—while neg did not start executing until it was awaited.

