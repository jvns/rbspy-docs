+++
title = "Using rbspy"
description = ""
keywords = []
categories = []
url = "/using-rbspy/"
+++

# Subcommands

`rbspy` has 3 subcommands: `snapshot`, `record`, and `report`.

## Snapshot

Snapshot takes a single stack trace from the specified process, prints it, and exits. This is
useful if you have a stuck Ruby program and just want to know what it's doing right now.  Must be
run as root.

```
sudo rbspy snapshot --pid $PID
```

## Record

Record records stack traces from your process and saves them to disk.

`rbspy record` will save 2 files: a gzipped raw data file, and a visualization (by default a flamegraph, you
can configure the visualization format with `--format`). The raw data file contains every stack
trace that `rbspy` recorded, so that you can generate other visualizations later if you want. By
default, rbspy saves both files to `~/.cache/rbspy/records`, but you can customize where those are
stored with `--file` and `--raw-file`.

This is useful when you want to know what functions your program is spending most of its time in.

You can record stack traces in two different ways, by PID or by executing a new ruby process.

### Record by PID

```
# Must be run as root
sudo rbspy record --pid $PID
```

### Record by executing the process through rbspy

```
# Must be run as root on Mac (but not Linux)
rbspy record ruby myprogram.rb
```

The reason this has to be run as root on Mac but not on Linux is that Mac and Linux systems APIs are
different. rbspy can use the `process_vm_readv` system call to read memory from a child process on
Linux without being root, but can't do the same with `vm_read` on Mac.

If run with sudo, `rbspy record` by default drops root privileges when executing a subprocess. So if
you're user `bork` and run `sudo rbspy record ruby script.rb`. You can disable this with
`--no-drop-root`.

### Optional Arguments

These work regardless of how you started the recording. 

 * `--rate`: Specifies the frequency of that stack traces are recorded. The interval is determined by `1000/rate`. The default rate is 100hz.
 * `--duration`: Specifies how long to run before stopping rbspy. This conficts with running a subcommand (`rbspy record ruby myprogram.rb`).
 * `--format`: Specifies what format to use to report profiling data. The options are:
   * `flamegraph`: generates a flamegraph SVG that you can view in a browser
   * `callgrind`: generates a callgrind-formatted file that you can view with a tool like
     `kcachegrind`.
   * `summary`: aggregates % self and % total times by function. Useful to get a basic overview
   * `summary_by_line`: aggregates % self and % total times by line number. Especially useful when
      there's 1 line in your program which is taking up all the time.
 * `--file`: Specifies where rbspy will save formatted output. 
 * `--raw-file`: Specifies where rbspy will save formatted data. Use a gz extension because it will be gzipped.

## Report

If you have a raw rbspy data file that you've previously recorded, you can use `rbspy report` to
generate different kinds of visualizations from it (the flamegraph/callgrind/summary formats, as
documented above). This is useful because you can record raw data from a program and then decide how
you want to visualize it afterwards.

For example, here's what recording a simple program and then generating a summary report looks like:

```
$ sudo rbspy record --raw-file raw.gz ruby ci/ruby-programs/short_program.rb
$ rbspy report -f summary -i raw.gz -o summary.txt
$ cat summary.txt
% self  % total  name
100.00   100.00  <c function> - unknown
  0.00   100.00  ccc - ci/ruby-programs/short_program.rb
  0.00   100.00  bbb - ci/ruby-programs/short_program.rb
  0.00   100.00  aaa - ci/ruby-programs/short_program.rb
  0.00   100.00  <main> - ci/ruby-programs/short_program.rb
```

# FAQ

## Who makes rbspy?

[Julia Evans](https://jvns.ca) started the project and is the primary maintainer.
For a full list of contributors, see [the github repo](https://github.com/rbspy/rbspy)

## Who funds rbspy?

Initial rbspy development was funded by [the Segment Open Fellowship](https://segment.com/blog/segment-open-fellowship-2017/) -- they paid for 3 months of
development on the project, to take it from a sketchy prototype to an actual working profiler that
people use to make their Ruby programs faster. Julia took a 3 month sabbatical off work to build it.

This kind of short-term funding is an awesome way to bootstrap new open source projects that might
not happen otherwise! You can do a lot in 3 months :)

## Can I use rbspy in production?

Yes! rbspy does not add any overhead to your Ruby programs -- it only reads memory from the Ruby
process you're monitoring, it doesn't make any changes. Unlike some other statistical profilers,
rbspy does **not** use signals or ptrace, so it won't interrupt system calls your Ruby program is
making.

The only two things to be aware of are:

* `rbspy` does use some CPU. If you use `rbspy record --subprocesses`, it can use a substantial
  amount of CPU (because it'll start a separate thread for every process it's recording)
* disk usage: `rbspy record` will save a data file to disk with compressed stack traces, and if you
  run it for many hours it's possible you'll use a lot of disk space. We recommend giving rbspy a
  time limit, like `rbspy record --duration 10`.

Any bugs in rbspy will manifest as rbspy crashing, not your Ruby program crashing.

## How does rbspy read data from my Ruby processes?

On Linux, it uses the `process_vm_readv` system call, which lets you read memory from any other
running process.

## Doesn't not pausing the Ruby process cause errors?

Yep!

rbspy does not stop your Ruby processes to collect information about what it's doing. This is for
both performance reasons and general production-safety reasons -- only **reading** from your Ruby
processes and not altering them in any way means that rbspy is safer to run on production Ruby
applications. rbspy does not use ptrace or signals.

This means that sometimes rbspy will try to read a stack trace out of a Ruby process, there will be
a race, and the memory of that process will temporarily be in an invalid state which means rbspy
can't collect its stack. `rbspy record` handles this by just dropping that stack trace and trying
again later.

## How does rbspy handle threads?

`rbspy` always collects the stack from what the Ruby VM reports as the currently running thread.
This is because the GIL only allows one thread to be running Ruby code at any given time. It ignores
threads that are not currently running.

## Can rbspy profile C extensions?

Not yet. Any calls into C will be reported as "unknown C function".

## Is there a similar project for Python?

Yes! [pyflame](https://github.com/uber/pyflame) by [Evan Klitzke](https://eklitzke.org/) does
basically the same thing as rbspy, but for Python programs.

## Who made the logo?

[Ashley McNamara](https://twitter.com/ashleymcnamara) was extremely kind and designed it!! She's an
awesome software engineer who also makes delightful stickers. See her 
[gophers repository](https://github.com/ashleymcnamara/gophers) for a bunch of awesome gopher art she's done
for the Go community.

