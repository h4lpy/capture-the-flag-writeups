# Ancient Fossil

All things are lost to time...

File: [ancient.fossil]()

-----

**Ancient Fossil** was a Forensics challenge released on Day #24 of the Huntress Capture the Flag (CTF) competition. We were provided a [Fossil repository file](https://fossil-scm.org/home/doc/hierarchical-manifests/www/fileformat.wiki), `ancient.fossil`, and tasked with finding the flag within the repository.

Using the `fossil` command-line tool, we see that there are 403 commits within the timeline:

```
$ sudo apt-get install fossil
$ fossil timeline -n 0 --type ci -R ancient.fossil | grep '^20' | wc -l  
403
```

We then used the `deconstruct` option to extract all of the file artifacts from the repository:

```
$ fossil deconstruct -R ancient.fossil .
```

With all of these artifacts now extracted, we then searched recursively for the flag via `grep`:

```
$ grep -R "flag" .
```

![](https://miro.medium.com/v2/resize:fit:700/1*WpBkr9XoS2MOhn89ZQhV9g.png)

Extracting artifacts via fossil and searching for the flag

```
flag{2ed33f365669ea9f10b1a4ea4566fe8c}
```

# References

Fossil. _Fossil File Formats_. [https://fossil-scm.org/home/doc/hierarchical-manifests/www/fileformat.wiki](https://fossil-scm.org/home/doc/hierarchical-manifests/www/fileformat.wiki)