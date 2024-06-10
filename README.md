# ln2

`ln2` is a drop-in replacement for `ln` that uses an intuitive, memorable
syntax to make links between files.

**Example**
```
$ ln2 -s $HOME/new_link \> $PWD/target_file
```

## Explanation

### Problem

I can never remember the correct argument order for the `ln` command. Call me
what you will, but I'm not the only one who's confused
\[[1](https://www.reddit.com/r/programming/comments/1qt0z/ln_s_d1_d2_am_i_the_only_person_who_gets_this_the/)\]
\[[2](https://news.ycombinator.com/item?id=1984456)\]. I've thought long and
hard about why I have this particular mental block and I've come to the
conclusion that I simply want to specify the arguments in the opposite order of
what's required. Maybe that's because whenever I want to inspect a link (using
`ls -l`), it's shown like this:

```
$ ls -l some_link
lrwxrwxrwx 2 msalvatore msalvatore 1 Jun  6 12:33 some_link -> real_file
```

That output makes a lot of sense. I think of links as files that point to other
files; the output of `ls` is consistent with my mental model of what a link is.
Intuitively, I want to specify the creation of links in the same way that I
inspect them (and understand them), and there is no mnemonic or memory device
that will change that [<img
src="https://www.wppluginsforyou.com/wp-content/uploads/2020/05/tooltips.png"
width="15px" />](## "Besides, I don't want to spend my mental cycles
using a mnemonic device every time I need to use `ln`. I'd prefer not to have
to think about it because it behaves the way I expect it to.").

To make matters worse, the man pages add more confusion.

```manpage
LN(1)                            User Commands                           LN(1)

NAME
       ln - make links between files

SYNOPSIS
       ln [OPTION]... [-T] TARGET LINK_NAME
```

"That seems pretty clear", I hear you say. I agree. The thing is that this is
the man page for GNU ln, which is what you get on Linux. Sometimes I use
FreeBSD or MacOS, and that man page looks like this:

```manpage
LN(1)                   FreeBSD General Commands Manual                  LN(1)

NAME
     ln, link – link files

SYNOPSIS
     ln [-L | -P | -s [-F]] [-f | -iw] [-hnv] source_file [target_file]
```

What?!?! For BSD ln the arguments are specified in the opposite order than for
GNU ln? Well, no, but you could be forgiven for thinking so. It's
just that in the BSD and the GNU man pages, the word "target" has opposite
meanings.

There has to be a better way.

### Solution

Introducing `ln2`, a drop-in replacement for `ln` that uses an intuitive,
memorable syntax for the creation of links.

**Example**
```
$ ln2 -s $HOME/new_link \> $PWD/target_file
```

Wow, that's unambiguous! Just like the output of the `ls` command, `ln2` allows
you think in terms of a link that "points to" a target file. If you prefer
the original argument ordering, `ln2` can accommodate your preference:

**Example**
```
$ ln2 -s $PWD/target_file \< $HOME/new_link
```

You no longer need to try to remember the correct order; an operator is used to
clearly indicate where a link should point. Confusing man pages with overloaded
terms are no longer a problem, either.

**NOTE**: The `>` and `<` operators must be escaped with a backslash (`\`),
otherwise the shell will interpret your command to mean the output of `ln2`
should be redirected to a file.

## Installation

1. Copy the [ln2](./ln2) script somewhere in your PATH. `/usr/local/bin` is
   the recommended location installation location.
    ```
    sudo curl -o /usr/local/bin/ln2 https://raw.githubusercontent.com/mssalvatore/ln2/main/ln2
    ```

1. Make `ln2` executable.
    ```
    sudo chmod 755 /usr/local/bin/ln2
    ```

1. Optionally, if you'd like to replace `ln` with `ln2`, create an alias:
    ```
    echo "alias ln=ln2 >> $HOME/.bashrc"
    ```

    You'll need to close and reopen your shell for this to take effect.

    To use the original `ln` command without disabling your alias, you can
    invoke it using a backslash:
    ```
    $ \ln ...
    ```

## Usage
```
$ln2 -h

Make links between files using an intuitive, memorable syntax.

Usage: ln2 [OPTION]... LINK_NAME \> TARGET
  or:  ln2 [OPTION]... TARGET \< LINK_NAME

       See LN(1) for a description of the available options.
```
