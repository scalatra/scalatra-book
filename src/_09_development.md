
Development Techniques
======================

Automatic Code Reloading
------------------------

Restarting an application manually after every code change is both slow and
painful. It can easily be avoided by using a tool for automatic code reloading.

### SBT

SBT will allow you to [signal a restart of the application when it detects
code changes](https://github.com/harrah/xsbt/wiki/Triggered-Execution). The
syntax for restarting includes adding `~` in front of the command you want to
re-execute.

Usage is rather simple:

{pygmentize::}
$ sbt
> container:start
> ~ compile
{pygmentize}

