# Challenge 02 | Log Search Tool

## Overview

After recent attacks, we’ve developed a search tool. Search the logs and discover what the attackers were after.

Hint: Always search deeper.

## Walkthrough

We begin with a web interface showing the log search tool.  From this, we can search through various files for specified terms

![[ep000_02_log_search_tool.png]]

Opening BurpSuite, we see a `src.txt` file within the **Target** section:

![[ep000_02_burpsuite_src_txt.png]]

Retrieving this file via `wget` and opening it in a text editor reveals it is written in Perl:

```perl
$ cat src.pl                        
use strict;
use warnings;

use utf8::all;
use CGI;
use HTML::Template;

sub files_in_dir {
  my $dirname = shift;
  opendir my $dir, $dirname;
  my @files = grep $_ ne "." && $_ ne "..", readdir $dir;
  closedir $dir;
  return @files;
}

sub find_lines {
  my ($filename, $needle) = @_;
  my @results = ();
  if (length($needle) >= 4) {
    # I am sure this is totally secure!
    open(my $fh, "logs/".$filename);
    while (my $line = <$fh>) {
      if (index(lc($line), lc($needle)) >= 0) {
        push(@results, $line);
      }
    }
  }
  return @results;
}

sub list_to_tmpl {
  my ($tmpl_name, @elems) = @_;
  my @results = ();
  while (@elems) {
    push(@results, {$tmpl_name, shift @elems});
  }
  return \@results;
}

sub main_page {
  my $tmpl = HTML::Template->new(filename => "templates/default.html");
  $tmpl->param(FILES => list_to_tmpl("NAME", files_in_dir("logs")));
  return $tmpl->output;
}

my $q = CGI->new;

my $pfile = $q->param("file");
if (($pfile // "") eq "") {
  print $q->header(-charset => "utf-8");
  print main_page;
} else {
  print $q->header(-type => "text/plain", -charset => "utf-8");
  print join("", find_lines($pfile, scalar $q->param("term")));
}
```

Interestingly, a developer has left a comment to indicate a potentially insecure practise:

```perl
# I am sure this is totally secure!
open(my $fh, "logs/".$filename);
```

After some research, it appears that the `open()` command allows us to execute arbitrary commands and even view files within other directories via local file inclusion.  For example, we can use BurpSuite to intercept the request and change the `file` and `term` parameters to view the `/etc/passwd` file by traversing up the directories:

![[ep000_02_etc_passwd.png]]

From the above, we use `/../../` to navigate to the root of the filesystem (`/`) and then go to `/etc/passwd`.  We then have to specify a term that as at least four characters in length - in this case, we use `bin/` which will then return all of the lines within `/etc/passwd` which contain that term.

From the official [Perl documentation](https://perldoc.perl.org/functions/open), we can append a pipe (`|`) to the end of our input to force the web app to interpret it as a command and output it to us.  We can use the following as our payload:

```
/../../usr/bin/ls -la | 
```

However, since we are passing this through URL parameters, we must encode the spaces as `%20`:

```
/../../usr/bin/ls%20-la%20|
```

This will execute `/usr/bin/ls` with the `-la` options within the current directory.  We then specify `nobody` as our `term` field to get the output:

![[ep000_02_ls_la_command.png]]

Now that we can execute arbitrary commands, we can view the contents of the filesystem and retrieve the flag.  Listing the contents of the root of the filesystem (`/`) reveals the flag:

![[ep000_02_flag_location.png]]

We can then exectue `cat` (within `/usr/bin/`) to view its contents:

![[ep000_02_flag.png]]

Flag: `https://h4ck1ng.google/solve/Y37_@N07h3r_P3r1_H@X0R`