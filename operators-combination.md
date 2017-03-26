#Operators combination
There are many operators out there that allows you to combine the values from 2 or more source, they act a bit differently though and its important to know the difference.

## combineLatest

The signature on this one is:
```
Rx.Observable.combineLatest([ source_1, ...  source_n])
```

```
let source1 = Rx.Observable.interval(100)
.map( val => "source1 " + val ).take(5);

let source2 = Rx.Observable.interval(50)
.map( val => "source2 " + val ).take(2);

let stream$ = Rx.Observable.combineLatest(
    source1,
    source2
);

stream$.subscribe(data => console.log(data));

// emits source1: 0, source2 : 0 |  source1 : 0, source2 : 1 | source1 : 1, source2 : 1, etc
```

What this does is to essentially take the latest response from each `source` and return it as an array of x number of elements. One element per source.

As you can see source2 stops emitting after 2 values but is able to keep sending the latest emitted. 

### Business case
The business case is when you are interested in the very latest from each source and past values is of less interest, and of course you have more than one source that you want to combine.

## concat
The signature is :

```
Rx.Observable([ source_1,... sournce_n ])
```

Looking at the following data, it's easy to think that it should care when data is emitted:

```
let source1 = Rx.Observable.interval(100)
.map( val => "source1 " + val ).take(5);

let source2 = Rx.Observable.interval(50)
.map( val => "source2 " + val ).take(2);


let stream$ = Rx.Observable.concat(
    source1, 
    source2
);

stream$.subscribe( data => console.log('Concat ' + data));

// source1 : 0, source1 : 1, source1 : 2, source1 : 3, source1 : 4
// source2 : 0, source2 : 1 
```
The result however is that the resulting observable takes all the values from the first source and emits those first then it takes all the values from source 2, so order in which the source go into `concat()` operator matters.

So if you have a case where a source somehow should be prioritized then this is the operator for you.


