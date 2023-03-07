# sbcl-web-backtrace-mem-crash
A demo of a memory corruption that occurs in SBCL when running hunchentoot, and doing a backtrace

 **CAVEAT** - $SBCLRC needs to point to a suitable startup file defining package locations
 
 eg: export SBCLRC=$HOME/.sbclrc

On at least one flavor of linux, starting this file as

  export RunBugServer=1 ; sbcl --script ./web-server-crash-test.lisp  
  
  

and in another sbcl simply loading this file and running

  (make-test-json-web-request \*good-json\*)  ==> OK response
  
and

  (make-test-json-web-request \*bad-json\*)  

will trigger a memory failure like the one that is detailed in the comments of the lisp code


This fails on CentOS Linux,
uname:
 Linux xxx.edu 3.10.0-1160.45.1.el7.x86_64 #1 SMP Wed Oct 13 17:20:51 UTC 2021 x86_64
x86_64 x86_64 GNU/Linux

SBCL 2.3.1

This might be because the backtrace can contain instances of #<SB-IMPL::STRING-OUTPUT-STREAM {xxxxx}> which has dynamic-extent, so the backtrace can touch junk items on the stack
