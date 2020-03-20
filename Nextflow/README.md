## Nextflow cheatsheet

Syntax
------

#### Nextflow uses C family comments
``` groovy
//ignore this comment
normal code
```

#### Printing values: values can be printed with or without parentheses
``` groovy
println('Hello, world!')
println 'Hello, world!'
```

#### Variables are defined using equals symbol
``` groovy
var1 = 'Hello, world!'
```


### Channels
------------
Nextflow channels are used to connect logical tasks to each other

There are diferent types of channels

#### Value channels aka singleton channels
Value channels can be output as many times as you want

``` groovy
p = Channel.value( [1,2,3,4,5] )
p.view()
p.view()
```

#### From channels
From channels act as queues with a FIFO structure

These are queue channels and can have one and exactly one producer and one and exactly one consumer.

This means that trying to view the channel twice will consume all the input (run out) and return an error.

```groovy
p = channel.from(1,2,3,4,5)
p.view()
//p.view() running this a second time will fail
```