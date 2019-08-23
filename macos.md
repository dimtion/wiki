MacOS
==============

Because we sometime need to work on the dark side...

## Package management

Use Brew and Brew cask.

## `ps f`

The MacOS version of `ps` does noot have a `-f` option. This can be emulated by using the `pstree` program  available in the `pstree` brew package.

```perl
#!/usr/bin/perl
# treeps -- show ps(1) as process hierarchy -- v1.0 erco@seriss.com 07/08/14
my %p; # Global array of pid info
sub PrintLineage($$) {    # Print proc lineage
  my ($pid, $indent) = @_;
  printf("%s |_ %-8d %s\n", $indent, $pid, $p{$pid}{cmd}); # print
  foreach my $kpid (sort {$a<=>$b} @{ $p{$pid}{kids} } ) {  # loop thru kids
    PrintLineage($kpid, "   $indent");                       # Recurse into kids
  }
}
# MAIN
open(FD, "ps axo ppid,pid,command|");
while ( <FD> ) { # Read lines of output
  my ($ppid,$pid,$cmd) = ( $_ =~ m/(\S+)\s+(\S+)\s(.*)/ );  # parse ps(1) lines
  $p{$pid}{cmd} = $cmd;
  # $p{$pid}{kids} = (); <- this line is not needed and can cause incorrect output
  push(@{ $p{$ppid}{kids} }, $pid); # Add our pid to parent's kid
}
PrintLineage(($ARGV[0]) ? $ARGV[0] : 1, "");     # recurse to print lineage starting with specified PID or PID 1.
```

Sources:

* https://apple.stackexchange.com/a/11806
* https://apple.stackexchange.com/a/137355


## CoreAudio

If there are sound problems, (low volume...) killing Core audio might be a solution:
```bash
sudo killall coreaudiod
sudo launchctl unload /System/Library/LaunchDaemons/com.apple.audio.coreaudiod.plist && sudo launchctl load /System/Library/LaunchDaemons/com.apple.audio.coreaudiod.plist
```
