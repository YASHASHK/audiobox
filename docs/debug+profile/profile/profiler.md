---
title: The Mono log profiler
redirect_from:
  - /Profiler/
  - /LoggingProfiler/
---

The Mono *log* profiler can be used to collect a lot of information about a program running in the Mono runtime. This data can be used (both while the process is running and later) to do analyses of the program behaviour, determine resource usage, performance issues or even look for particular execution patterns.

This is accomplished by logging the events provided by the Mono runtime through the profiling interface and periodically writing them to a file which can be later inspected with the command line *mprof-report* program or with a GUI (not developed yet).

The events collected include (among others):

-   method enter and leave
-   object allocation
-   garbage collection
-   JIT compilation
-   metadata loading
-   lock contention
-   exceptions

In addition, the profiler can periodically collect info about all the objects present in the heap at the end of a garbage collection (this is called heap shot and currently implemented only for the sgen garbage collector). Another available profiler mode is the *sampling* or *statistical* mode: periodically the program is sampled and the information about what the program was busy with is saved. This allows to get information about the program behaviour without degrading its performance too much (usually less than 10%).

Basic profiler usage
--------------------

The simpler way to use the profiler is the following:

`mono --profile=log program.exe`

At the end of the execution the file *output.mlpd* will be found in the current directory. A summary report of the data can be printed by running:

`mprof-report output.mlpd`

With this invocation a huge amount of data is collected about the program execution and collecting and saving this data can significantly slow down program execution. If saving the profiling data is not needed, a report can be generated directly with:

`mono --profile=log:report program.exe`

If the information about allocations is not of interest, it can be excluded:

`mono --profile=log:noalloc program.exe`

On the other hand, if method call timing is not important, while allocations are, the needed info can be gathered with:

`mono --profile=log:nocalls program.exe`

You will still be able to inspect information about the sequence of calls that lead to each allocation because at each object allocation a stack trace is collected if full enter/leave information is not available.

To periodically collect heap shots (and exclude method and allocation events) use the following options (making sure you run with the sgen garbage collector):

`mono --gc=sgen --profile=log:heapshot program.exe`

To perform a sampling profiler run, use the *sample* option:

`mono --profile=log:sample program.exe`

Profiler option documentation
-----------------------------

By default the *log* profiler will gather all the events provided by the Mono runtime and write them to a file named *output.mlpd*. When no option is specified, it is equivalent to using:

`--profile=log:calls,alloc,output=output.mlpd,maxframes=8,calldepth=100`

The following options can be used to modify this default behaviour. Each option is separated from the next by a `,` character, with no spaces and all the options are included after the *log:* profile module specifier.

-   *help*: display concise help info about each available option
-   *[no]alloc*: *noalloc* disables collecting object allocation info, *alloc* enables it if it was disabled by another option like *heapshot*.
-   *[no]calls*: *nocalls* disables collecting method enter and leave events. When this option is used at each object allocation and at some other events (like lock contentions and exception throws) a stack trace is collected by default. See the *maxframes* option to control this behaviour. *calls* enables method enter/leave events if they were disabled by another option like *heapshot*.
-   *heapshot[=MODE]*: collect heap shot data at each major collection. The frequency of the heap shots can be changed with the *MODE* parameter. When this option is used allocation events and method enter/leave events are not recorded by default: if they are needed, they need to be enabled explicitly. The optional parameter *MODE* can modify the default heap shot frequency. heapshot can be used multiple times with different modes: in that case a heap shot is taken if either of the conditions are met. MODE can be one of:
    -   *NUM*ms: perform a heap shot if at least *NUM* milliseconds passed since the last one.
    -   *NUM*gc: perform a heap shot every *NUM* major garbage collections
    -   *ondemand*: perform a heap shot when such a command is sent to the control port

-   *sample[=TYPE[/FREQ]]*: collect statistical samples of the program behaviour. The default is to collect a 1000 times per second the instruction pointer. This is equivalent to the value "cycles/1000" for *TYPE*. On some systems, like with recent Linux kernels, it is possible to cause the sampling to happen for other events provided by the performance counters of the cpu. In this case, *TYPE* can be one of:
    -   *cycles*: processor cycles
    -   *instr*: executed instructions
    -   *cacherefs*: cache references
    -   *cachemiss*: cache misses
    -   *branches*: executed branches
    -   *branchmiss*: mispredicted branches

-   *time=TIMER*: use the TIMER timestamp mode. TIMER can have the following values:
    -   *fast*: a usually faster but possibly more inaccurate timer

-   *maxframes=NUM*: when a stack trace needs to be performed, collect *NUM* frames at the most. The default is 8.
-   *calldepth=NUM*: ignore method enter/leave events when the call chain depth is bigger than NUM.
-   *zip*: automatically compress the output data in gzip format.
-   *output=OUTSPEC*: instead of writing the profiling data to the output.mlpd file, substitute *%p* in *OUTSPEC* with the current process id and *%t* with the current date and time, then do according to *OUTSPEC*:
    -   if *OUTSPEC* begins with a *|* character, execute the rest as a program and feed the data to its standard input
    -   if *OUTSPEC* begins with a *-* character, use the rest of OUTSPEC as the filename, but force overwrite any existing file by that name
    -   otherwise write the data the the named file: note that is a file by that name already exists, a warning is issued and profiling is disabled.

-   *report*: the profiling data is sent to mprof-report, which will print a summary report. This is equivalent to the option: `output=mprof-report -`. If the *output* option is specified as well, the report will be written to the output file instead of the console.
-   *port=PORT*: specify the tcp/ip port to use for the listening command server. Currently not available for windows. This server is started for example when heapshot=ondemand is used: it will read commands line by line. The following commands are available:
    -   *heapshot*: perform a heapshot as soon as possible

Analyzing the profile data
--------------------------

Currently there is a command line program (*mprof-report*) to analyze the data produced by the profiler. This is ran automatically when the *report* profiler option is used. Simply run:

`mprof-report output.mlpd`

to see a summary report of the data included in the file.

### Trace information for events

Often it is important for some events, like allocations, lock contention and exception throws to know where they happened. Or we may want to see what sequence of calls leads to a particular method invocation. To see this info invoke mprof-report as follows:

`mprof-report --traces output.mlpd`

The maximum number of methods in each stack trace can be specified with the *--maxframes=NUM* option:

`mprof-report --traces --maxframes=4 output.mlpd`

The stack trace info will be available if method enter/leave events have been recorded or if stack trace collection wasn't explicitly disabled with the *maxframes=0* profiler option. Note that the profiler will collect up to 8 frames by default at specific events when the *nocalls* option is used, so in that case, if more stack frames are required in mprof-report, a bigger value for maxframes when profiling must be used, too.

The *--traces* option also controls the reverse reference feature in the heapshot report: for each class it reports how many references to objects of that class come from other classes.

### Sort order for methods and allocations

When a list of methods is printed the default sort order is based on the total time spent in the method. This time is wall clock time (that is, it includes the time spent, for example, in a sleep call, even if actual cpu time would be basically 0). Also, if the method has been ran on different threads, the time will be a sum of the time used in each thread.

To change the sort order, use the option:

`--method-sort=MODE`

where *MODE* can be:

-   *self*: amount of time spent in the method itself and not in its callees
-   *calls*: the number of method invocations
-   *total*: the total time spent in the method.

Object allocation lists are sorted by default depending on the total amount of bytes used by each type.

To change the sort order of object allocations, use the option:

`--alloc-sort=MODE`

where *MODE* can be:

-   *count*: the number of allocated objects of the given type
-   *bytes*: the total number of bytes used by objects of the given type

### Selecting what data to report

The profiler by default collects data about many runtime subsystems and mprof-report prints a summary of all the subsystems that are found in the data file. It is possible to tell mprof-report to only show information about some of them with the following option:

`--reports=R1[,R2...]`

where the report names R1, R2 etc. can be:

-   *header*: information about program startup and profiler version
-   *jit*: JIT compiler information
-   *sample*: statistical sampling information
-   *gc*: garbage collection information
-   *alloc*: object allocation information
-   *call*: method profiling information
-   *metadata*: metadata events like image loads
-   *exception*: exception throw and handling information
-   *monitor*: lock contention information
-   *thread*: thread information
-   *heapshot*: live heap usage at heap shots

It is possible to limit some of the data displayed to a timeframe of the program execution with the option:

`--time=FROM-TO`

where *FROM* and *TO* are seconds since application startup (they can be floating point numbers).

Another interesting option is to consider only events happening on a particular thread with the following option:

`--thread=THREADID`

where *THREADID* is one of the numbers listed in the thread summary report (or a thread name when present).

By default long lists of methods or other information like object allocations are limited to the most important data. To increase the amount of information printed you can use the option:

`--verbose`

### Track individual objects

Instead of printing the usual reports from the profiler data, it is possible to track some interesting information about some specific object addresses. The objects are selected based on their address with the *--track* option as follows:

`--track=0xaddr1[,0xaddr2,...]`

The reported info (if available in the data file), will be class name, size, creation time, stack trace of creation (with the *--traces* option), etc. If heapshot data is available it will be possible to also track what other objects reference one of the listed addresses.

The object addresses can be gathered either from the profiler report in some cases (like in the monitor lock report), from the live application or they can be selected with the *--find=FINDSPEC* option. FINDSPEC can be one of the following:

-   *S:SIZE*: where the object is selected if it's size is at least *SIZE*
-   *T:NAME*: where the object is selected if *NAME* partially matches its class name

This option can be specified multiple times with one of the different kinds of FINDSPEC. For example, the following:

`--find=S:10000 --find=T:Byte[]`

will find all the byte arrays that are at least 10000 bytes in size.

Note that with a moving garbage collector the object address can change, so you may need to track the changed address manually. It can also happen that multiple objects are allocated at the same address, so the output from this option can become large.

### Saving a profiler report

By default mprof-report will print the summary data to the console. To print it to a file, instead, use the option:

`--out=FILENAME`

Dealing with profiler slowness
------------------------------

If the profiler needs to collect lots of data, the execution of the program will slow down significantly, usually 10 to 20 times slower. There are several ways to reduce the impact of the profiler on the program execution.

### Use the statistical sampling mode

Statistical sampling allows executing a program under the profiler with minimal performance overhead (usually less than 10%). This mode allows checking where the program is spending most of it's execution time without significantly perturbing its behaviour.

### Collect less data

Collecting method enter/leave events can be very expensive, especially in programs that perform many millions of tiny calls. The profiler option *nocalls* can be used to avoid collecting this data or it can be limited to only a few call levels with the *calldepth* option.

Object allocation information is expensive as well, though much less than method enter/leave events. If it's not needed, it can be skipped with the *noalloc* profiler option. Note that when method enter/leave events are discarded, by default stack traces are collected at each allocation and this can be expensive as well. The impact of stack trace information can be reduced by setting a low value with the *maxframes* option or by eliminating them completely, by setting it to 0.

The other major source of data is the heapshot profiler option: especially if the managed heap is big, since every object needs to be inspected. The *MODE* parameter of the *heapshot* option can be used to reduce the frequency of the heap shots.

### Reduce the timestamp overhead

On many operating systems or architectures what actually slows down profiling is the function provided by the system to get timestamp information. The *time=fast* profiler option can be usually used to speed up this operation, but, depending on the system, time accounting may have some level of approximation (though statistically the data should be still fairly valuable).

Dealing with the size of the data files
---------------------------------------

When collecting a lot of information about a profiled program, huge data files can be generated. There are a few ways to minimize the amount of data, for example by not collecting some of the more space-consuming information or by compressing the information on the fly or by just generating a summary report.

### Reducing the amount of data

Method enter/leave events can be excluded completely with the *nocalls* option or they can be limited to just a few levels of calls with the *calldepth* option. For example, the option:

`calldepth=10`

will ignore the method events when there are more than 10 managed stack frames. This is very useful for programs that have deep recursion or for programs that perform many millions of tiny calls deep enough in the call stack. The optimal number for the calldepth option depends on the program and it needs to be balanced between providing enough profiling information and allowing fast execution speed.

Note that by default, if method events are not recorded at all, the profiler will collect stack trace information at events like allocations. To avoid gathering this data, use the *maxframes=0* profiler option.

Allocation events can be eliminated with the *noalloc* option.

Heap shot data can also be huge: by default it is collected at each major collection. To reduce the frequency, you can specify a heapshot mode: for example to collect every 5 collections (including major and minor):

`heapshot=5gc`

or when at least 5 seconds passed since the last heap shot:

`heapshot=5000ms`

### Compressing the data

To reduce the amout of disk space used by the data, the data can be compressed either after it has been generated with the gzip command:

`gzip -9 output.mlpd`

or it can be compressed automatically by using the *zip* profiler option. Note that in this case there could be a significant slowdown of the profiled program.

The mprof-report program will tranparently deal with either compressed or uncompressed data files.

### Generating only a summary report

Often it's enough to look at the profiler summary report to diagnose an issue and in this case it's possible to avoid saving the profiler data file to disk. This can be accomplished with the *report* profiler option, which will basically send the data to the mprof-report program for display.

To have more control of what summary information is reported (or to use a completely different program to decode the profiler data), the *output* profiler option can be used, with `|` as the first character: the rest of the output name will be executed as a program with the data fed in on the standard input.

For example, to print only the Monitor summary with stack trace information, you could use it like this:

`output=|mprof-report --reports=monitor --traces -`

### Editing this page

This page is automatically generated from the log profiler information in git. Make sure any updates are done in the master version.
