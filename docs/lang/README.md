title: Overview

# Coding Languages

This part will work as an introduction to each of the used languages. It gives you a short introduction
but to fully understand the language you should also read the referenced documents and try it out some days.

I used all of the following languages but neither do I currently use them on and on. See my [Blog](../blog) for where my programming activities are.
Which of the languages to use depends on the project and its goals. For the time of writing my preferences are JavaScript for client and for server if development speed matters or Rust if stability and performance matters.

The Languages are parted into:

## Markup and Templates

-   [Markdown](markdown.md)
-   [Handlebars](handlebars.md)
-   [Sass](sass.md)

## Scripting Languages

I use interpreted languages for fast solutions:

-   [Bash](bash.md)
-   [JavaScript](../js/README.md)
-   [TypeScript](../js/typescript.md) which is an optimization of JavaScript
-   [CoffeeScript](../js/coffee.md)

But then a project get's really big, it's attractive veneer quickly washes away.

## Compiler Languages

Here the compiler languages are coming back:

-   [Go](go.md)
-   [Rust](../rust)

You need more time to write code but it comes out as more robust and production ready solutions.

**Java** is missing here completely while I know it and also made a big project with it I believe that this language is neither the best solution. Often such applications need a lot of resources are not really fast and often a nightmare to find problems with `NullPointer` within. It's not generally this way but the code I had to manage tends this way.

## Comparison

Keep in mind that comparisons are always wrong because they only look at one very specific setup. But they may show some differences in technology.

### Web server

As I want to work on a web project which needs a stable and fast web server
I made an application serving a static HTML file in different languages. I did not
program everything on my own but used the most popular and stable modules for
all languages.

As a result I compare them against a plain Apache 2.4 installation. To make
it comparable all serve the same very simple HTML file which they will found in
their base directory.

I did run all three directly after each other on the same machine (i7 4CPU Cores,
8GB RAM).

**Apache 2.4**

    $ wrk -t4 -c400 -d30s http://127.0.0.1
    Running 30s test @ http://127.0.0.1
      4 threads and 400 connections
      Thread Stats   Avg      Stdev     Max   +/- Stdev
        Latency     3.14ms   10.29ms 346.49ms   98.67%
        Req/Sec     3.51k     2.97k   13.96k    63.27%
      363271 requests in 30.04s, 115.78MB read
    Requests/sec:  12094.09
    Transfer/sec:      3.85MB

    load average: 8,36, 4,16, 2,35

**Go using net/http**

    $ wrk -t4 -c400 -d30s http://127.0.0.1:8080
    Running 30s test @ http://127.0.0.1:8080
      4 threads and 400 connections
      Thread Stats   Avg      Stdev     Max   +/- Stdev
        Latency    24.62ms   42.73ms 634.52ms   89.12%
        Req/Sec    12.86k     3.38k   25.59k    75.25%
      1537844 requests in 30.09s, 393.03MB read
      Non-2xx or 3xx responses: 467
    Requests/sec:  51104.56
    Transfer/sec:     13.06MB

    load average: 4,69, 3,83, 2,37

**Rust using Iron**

    $ wrk -t4 -c400 -d30s http://127.0.0.1:8080

    Running 30s test @ http://127.0.0.1:8080
      4 threads and 400 connections
      Thread Stats   Avg      Stdev     Max   +/- Stdev
        Latency   406.52us  463.76us  72.85ms   95.36%
        Req/Sec    39.20k    36.83k   78.90k    55.17%
      2340369 requests in 30.06s, 412.91MB read
    Requests/sec:  77864.45
    Transfer/sec:     13.74MB

    load average: 14,28, 6,16, 3,25

**NodeJS using http-server**

    Running 30s test @ http://127.0.0.1:8080
      4 threads and 400 connections
      Thread Stats   Avg      Stdev     Max   +/- Stdev
        Latency    67.42ms   12.20ms 300.07ms   74.27%
        Req/Sec     1.47k   417.43     2.02k    45.78%
      174900 requests in 30.05s, 56.04MB read
    Requests/sec:   5820.36
    Transfer/sec:      1.87MB

    load average: 1.41 0.93 0.97

Apache did a low number of requests, Go was about 4 times faster but failed
for some requests. The most requests were answered by Rust with no errors. Also
the latency was very low but the system on full load. NodeJS as scripting language got the lowest number of request per sec but working only on one CPU it should also be better than apache if 4 cores will be used in a cluster mode. But the system load was the lowest, too.

So if performance matters Rust is the best to go but if only the pure static delivery is needed Nginx & Co should be checked against an own implementation.

See a complete [web server benchmarking](https://www.techempower.com/benchmarks/#section=data-r16&hw=cl&test=json&l=h7atq7).


{!docs/abbreviations.txt!}

