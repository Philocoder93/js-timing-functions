# Timing Functions

- `setInterval`: Returns a new "timer" object. Side-effect: starts the timer.
- `clearInterval`: Stops a timer object.

## The wrong way:

```js
var stopwatchBucket;
function sayHi(){
  console.log("hi");
}
$("#start").click(function(){
  stopwatchBucket = setInterval(sayHi, 1000);
});
$("#stop").click(function(){
  clearInterval(stopwatchBucket);
});
```

### Analogy

When you press the "start" button you create a stopwatch, you start it running, and you put that stopwatch in a bucket.

When you press the "start" button again, you create a new stopwatch, dump the old stopwatch out of the bucket onto the floor, and put the new stopwatch in the bucket.

When you press the "stop" button, it clears the stopwatch in the bucket. All the other stopwatches on the floor continue to run.

So if you press the "start" button 50 times, and then press "stop", it'll only reset the 50th stopwatch because that's the one in the bucket. There'll be 49 other stopwatches ticking merrily away on the floor.

### What's happening

`stopwatchBucket = setInterval(sayHi, 1000)` is overwriting the current value of the `stopwatchBucket` variable. It's exactly the same as this:

```js
var name = "Alice";
name = "Bob";
name = "Carol";
console.log(name);
// prints "Carol"
```

`clearInterval(stopwatchBucket)` is stopping whatever timer is currently in the `stopwatchBucket` variable. There is only ever one timer at a time in the variable. To have multiple, it would need to be an array.

## The right way:

```js
var stopwatchBucket = null;
function sayHi(){
  console.log("hi");
}
$("#start").click(function(){
  if(stopwatchBucket === null){
    console.log("Creating a new stopwatch and putting it in the bucket!");
    stopwatchBucket = setInterval(sayHi, 1000);
  }else{
    console.log("Not doing anything because there's already a stopwatch in the bucket.");
  }
});
$("#stop").click(function(){
  if(stopwatchBucket !== null){
    console.log("Stopping whatever's in the bucket, and emptying the bucket.");
    clearInterval(stopwatchBucket);
    stopwatchBucket = null;
  }else{
    console.log("Not doing anything because there's nothing in the bucket.");
  }
});
```

# Common Errors

```js
function sayHi(){
  console.log("hi");
}
setInterval(sayHi(), 1000);
```

The `()` after `sayHi` means it is called immediately and only once, rather than after every 1000 miliseconds.

---

```js
var timer = setInterval(sayHi, 1000);
clearInterval(sayHi);
```

You must pass the timer object itself into `clearInterval` -- not the function the timer object is running.


