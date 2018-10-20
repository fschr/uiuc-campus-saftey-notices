# UIUC Campus Safety Notices Toolkit

This repository contains a number of minimal tools
which can be used to fetch and parse information about
[UIUC campus safety notices](https://blogs.illinois.edu/view/7513).

The tools most useful to you are probably `csvify`,
`curl_historical_data`, and `fetch_long_descriptions`.

`csvify` converts HTML pages listing campus safety
notices to CSVs with the header
`id,date,short_description,link`. For example,

```
$ cat *.html | ./csvify
```

`curl_historical_data` downloads all the HTML pages
which list campus safety notices. If you read the
source code of the program, you'll notice it's a simple
hardcoded loop --- this needs to be replaced with
something more robust.

`fetch_long_descriptions` takes as input CSVs (like the
kind output by `csvify`) and writes the long
description of each safety notice to a file in the
`descriptions/` directory, where the file name
corresponds to the ID of the campus safety notice, as
listed in the CSVs.

## Extracting Attributes of Crimes

There are many ways to extract attributes of crimes
from this data. A very simple approach is to search the
long descriptions with `grep`. For example, to
determine the fraction of crimes which were sexual
assaults, grep for the word 'sexual':

```
$ grep 'sexual' descriptions/*.txt --color=always | less -R
```

*The `--color=always` flag makes the output easier to
read. Piping the output to `less` allows for the
perusal of grep output, without it blowing up your
terminal. The `-R` flag to `less` maintains the color
of the grep output.*

It's easy to skim this output and verify that the
crimes were, in fact, sexual assaults. Now, we must
count the total number of them and compare it to the
total number of crimes.

`grep` outputs each line which
has an occurence of the term 'sexual' and each file in
descriptions/ is exactly one line long; the problem of
counting the number of sexual assaults is reduced to
the problem of counting the number of lines of grep
output:

```
$ grep 'sexual' descriptions/*.txt | wc -l
25
```

We see that 25 of the crimes were sexual assaults. Now,
compare to the total number of crimes, which is
equivalent to the total number of files in the
descriptions/ directory:

```
$ ls -l descriptions/ | wc -l
57
```

Wow! Nearly half of all the campus safety notices (that
I had downloaded at the time of writing this README)
were sexual assaults.

Of course, this method of analysis is only a
heuristic. To determine the exact number of crimes
which were sexual assaults, one would have to manually
read the description of each.

## License

GNU AFFERO GENERAL PUBLIC LICENSE
