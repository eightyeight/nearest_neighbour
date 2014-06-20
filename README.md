# k-NN Algorithm in Go, Haskell, Rust, F\#, OCaml, and Factor

This repo contains naive implementations of the [k-NN algorithm](http://en.wikipedia.org/wiki/K-nearest_neighbors_algorithm) in several languages (for k = 1), and an extra one in Golang with some optimizations.

## Blog Chain

This was inspired by a chain of blog posts, the [last one](http://re-factor.blogspot.ca/2014/06/comparing-k-nn-in-factor.html) implemented it in [Factor](http://factorcode.org/).  Before that, [this one](http://huonw.github.io/2014/06/10/knn-rust.html) did it in [Rust](http://www.rust-lang.org/).  That one was inspired by [a blog post](http://philtomson.github.io/blog/2014/05/29/comparing-a-machine-learning-algorithm-implemented-in-f-number-and-ocaml/) which did it in [F#](http://fsharp.org/) and [OCaml](http://ocaml.org/), and [a follow-up](http://philtomson.github.io/blog/2014/05/30/stop-the-presses-ocaml-wins/) which improves the first OCaml implementation.  This repo adds the naive implementation in [Go](http://golang.org) and [Haskell](http://www.haskell.org/), and an additional implementation in Golang with some optimizations.

## Comparison

Performance comparisons were performed on a freshly spun up c3.xlarge EC2 instance.  Golang, Haskell, Rust, F# and OCaml were freshly installed, and Factor was freshly downloaded.  Code for the languages other than Go and Haskell were copy-pasta'd from those blog posts.  Executable binaries were compiled for Rust, OCaml, and F#, and run on the machine.  Here's a performance comparison summary, from fastest to slowest (naive implementations only, the optimized Golang implementation is not included in this comparison):

* Golang: 3.467s
* Factor: 6.358s
* OCaml: 12.757s
* F#: 23.507s
* Rust: 78.138s
* Haskell: 91.581s

The naive implementations are all essentially the same algorithm, but by some luck, the Golang implementation does a bit better in terms of accuracy (95% vs. 94.4% for all the others).  The Go implementation takes each member of the test sample, and in case there's a tie amongst members of the training sample vying to be the test member's nearest neighbour, it picks the first one ("first" meaning "occurring higher up in 'validationsample.csv'").  The other languages aren't transparent enough for me to tell immediately how they're picking, but maybe they don't pick as deterministically?

#### Golang
```
$ time go run golang-k-nn.go

475

real	0m3.467s
user	0m3.367s
sys	0m0.111s
```

#### Haskell
```
$ time ./haskell-k-nn

Percentage correct: 472

real  1m31.581s
user  1m29.191s
sys 0m2.384s
```

#### Rust
```
$ time ./rust-k-nn

Percentage correct: 94.4%

real	1m18.138s
user	1m17.980s
sys	0m0.155s
```

#### OCaml
```
$ time ./ocaml-k-nn

Percentage correct:94.400000

real	0m12.757s
user	0m12.500s
sys	0m0.257s
```

#### F#
```
$ time ./fsharp-k-nn.exe

start...
Percentage correct:94.400000

real	0m23.507s
user	0m22.751s
sys	0m0.798s
```

#### Factor
```
$ mkdir -p $FACTOR_HOME/work/k-nn
$ cp k-nn.factor $FACTOR_HOME/work/k-nn
$ cp *.csv $FACTOR_HOME/work/k-nn
$ $FACTOR_HOME/factor

IN: scratchpad USE: k-nn
Loading resource:work/k-nn/k-nn.factor
Loading resource:basis/formatting/formatting.factor
Loading resource:basis/formatting/formatting-docs.factor

IN: scratchpad gc [ k-nn ] time
Percentage correct: 94.400000
Running time: 6.357621145 seconds
```

## Optimized implementation in Golang

write this betteer
two optimizations
concurrency
short circuit bad distance calculations
run on ec2
show time

## TODO

* Write a blog post
* Do it in C++
* Do it in Python with [scikit-learn](http://scipy-lectures.github.io/advanced/scikit-learn/)

