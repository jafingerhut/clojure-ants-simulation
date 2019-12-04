# Introduction

Rich Hickey gave a talk in 2008 called "Clojure Concurrency", where he
described Clojure concurrency features, and walked through the source
code of a demo "ants colony" program, which demonstrates some of those
features.  Here is a [transcript of the "Clojure Concurrency"
talk](https://github.com/matthiasn/talk-transcripts/blob/master/Hickey_Rich/ClojureConcurrency.md),
containing a link to the video.

Here is a recording of [Julian Gamble's talk "Applying the paradigms
of core.async in
ClojureScript"](https://www.youtube.com/watch?v=JUrOebC5HmA),
presented at the Clojure Conj conference in 2014, where he shows this
demo running, and also an adaptation of the code to use Clojure's
core.async library.

[Here is a blog
post](https://juliangamble.com/blog/2014/12/31/clojure-ants-in-clojurescript-demo/)
by Julian about this.


# What is in this repository?

This repository contains Rich's original code (or at least I am
unaware of any changes between this version and the original code) in
the file [src/ants.clj](src/ants.clj).

A small variant of that code is in the file
[src/demo/AntsApplet2.clj](src/demo/AntsApplet2.clj), with a small
separate file
[src/demo/AntsAppletRunner.clj](src/demo/AntsAppletRunner.clj) with
the needed "wrapper code" to make it runnable as an applet in a web
browser that supports Java applets.


# How do I run it?

As of 2019, if you click on the "Get Started!" link on the [Clojure
site's home page](https://clojure.org), there are instructions for a
macOS or Linux system for "Clojure install and CLI tools".  You may
need to install Java as well.

For example, on an Ubuntu Linux system, if you type the command `java`
in a terminal, you should see output similar to what is shown below,
if Java is not already installed.

```
Command 'java' not found, but can be installed with:

sudo apt install default-jre              # version 2:1.11-72, or
sudo apt install openjdk-11-jre-headless  # version 11.0.5+10-0ubuntu1
sudo apt install openjdk-13-jre-headless  # version 13+33-1
sudo apt install openjdk-14-jre-headless  # version 14~18-1
sudo apt install openjdk-8-jre-headless   # version 8u232-b07-2ubuntu1
```

Installing Java 8 or 11 are good choices, if either of those are
available, e.g. on Ubuntu Linux:

```
$ sudo apt-get install openjdk-11-jre-headless
```

If you successfully follow those instructions, then the following
sample session in a terminal shows a few commands, and sample output
that should appear.  The prompt in the sample below is `$ `, which may
be different on your system.  Everything on a line after that is what
you need type, or copy/paste:

```bash
$ git clone https://github.com/jafingerhut/clojure-ants-simulation
[ ... output of git command omitted ... ]

$ cd clojure-ants-simulation
$ clojure
Clojure 1.10.1

[The very first time you do this, some extra lines of output may
appear here, indicating that the `clojure` command is downloading
Clojure and a few packages it depends upon.]

user=> 
```

`user=> ` is the default prompt of a Clojure REPL, when started with
`user` as the current namespace name.  All of the Clojure expressions
below that you should enter appear near the end of the file
[src/ants.clj](src/ants.clj), and you can copy/paste them from there
if you prefer (except the first, which in the original file contains a
full path to where that file existed on Rich Hickey's system when he
gave the demo).

```clojure
user=> (load-file "src/ants.clj")
nil

[If you run with Java version 11, you may see some scary-looking
warnings output at this point, which you can ignore.  You can see a
sample of similar warning messages, and why they appear if you are
curious, here:

https://clojure.org/guides/faq#illegal_access ]

[A new window should appear, which has a blue rectangle on a white
backgroun.  This is the "home" of the ants, where they begin the
simulation, and where they will drop food they find, if they are able
to return home while carrying food.]

user=> (def ants (setup))
#'user/ants

user=> (send-off animator animation)
object[clojure.lang.Agent 0x5d465e4b {:status :ready, :val nil}]

[In the new window that appeared above, now there should be many red
dots, representing food that the ants will search for, and many short
straight lines inside of the blue rectangle, representing ants.]

user=> (dorun (map #(send-off % behave) ants))
nil

[In the new window, the lines representing the ants should now be
moving around, foraging for food and trying to drop it back at home.
They drop "pheromones", drawn as green of varying intensities against
the white background.]

user=> (send-off evaporator evaporation)
#object[clojure.lang.Agent 0x1d71006f {:status :ready, :val nil}]

[The ants should continue moving, but now the green pheromones should
very gradually diminish in intensity, as a result of changes made by
the evaporation agent.]
```

You can quit the Clojure REPL session by typing Control-D, or entering
the expression `(System/exit 0)`.
