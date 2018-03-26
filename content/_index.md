# rbspy

Have you ever wanted to know what functions your Ruby program is calling? `rbspy` can tell you!

`rbspy` lets you profile Ruby processes that are already running. You give it a PID, and it starts
profiling! It's a sampling profiler, which means it's **low overhead** and **safe to run in
production**.

`rbspy` lets you record profiling data, save the raw profiling data to disk, and then analyze it in
a variety of different ways later on.

## Quickstart

1. Download the latest release from [the releases page](https://github.com/rbspy/rbspy/releases)
2. Unzip + move the rbspy binary to `/usr/local/bin/rbspy`
3. Profile a running Ruby process!

```
$ rbspy record --pid $PID
```

Here's what running `rbspy record` on a Rubocop process looks like. You'll see a live summary of
what the top functions being run are, and it also saves the raw data + a flamegraph for more in
depth analysis.

<img src="/rbspy-record.gif">
