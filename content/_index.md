# rbspy

Have a running Ruby program that you want to profile without restarting it? Want to profile a Ruby
command line program really easily? You want `rbspy`! rbspy can profile any Ruby program by running
1 command.

`rbspy` lets you profile Ruby processes that are already running. You give it a PID, and it starts
profiling. It's a sampling profiler, which means it's **low overhead** and **safe to run in
production**.

`rbspy` lets you record profiling data, save the raw profiling data to disk, and then analyze it in
a variety of different ways later on.

## Quickstart

Here's how to get started with rbspy:

1. Download the latest release from [the releases page](https://github.com/rbspy/rbspy/releases)
2. Unzip + move the rbspy binary to `/usr/local/bin/rbspy`
3. Profile a running Ruby process!

To start profiling, give `rbspy` the PID of the process you want to profile, like this:

```
$ rbspy record --pid $PID
```

You can also use `rbspy` to profile a Ruby script, like this. It works both with and without `bundle exec`.

```
$ rbspy record bundle exec ruby my-script.rb
```

Here's what running `rbspy record` on a Rubocop process looks like. You'll see a live summary of
what the top functions being run are, and it also saves the raw data + a flamegraph for more in
depth analysis.

![rbspy record demo](rbspy-record.gif)
