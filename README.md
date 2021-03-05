A hackday demo to demonstrate how it would be possible to enforce in-house custom coding standards of the kind that we discussed on this slack thread: https://cloudnc.slack.com/archives/G6S2H6ALR/p1614858248146800

You, uh, need Jai but if you _have_ it here is some demo output when none of the coding standards are violated:

```
matija@chip:~/dev/jai-coding-standards-demo (master)$ jai-linux build.jai 
Running linker: /home/matija/Dropbox/jai//bin/lld-linux -flavor Gnu --eh-frame-hdr -export-dynamic -o target /home/matija/dev/jai-coding-standards-demo/.build/target_3_0.obj /home/matija/dev/jai-coding-standards-demo/.build/target_3_1.obj /home/matija/dev/jai-coding-standards-demo/.build/target_3_2.obj /home/matija/dev/jai-coding-standards-demo/.build/target_3_3.obj /usr/lib/x86_64-linux-gnu/crt1.o /usr/lib/x86_64-linux-gnu/crti.o /usr/lib/x86_64-linux-gnu/crtn.o -L /home/matija/dev/jai-coding-standards-demo --dynamic-linker /lib64/ld-linux-x86-64.so.2 -rpath='$ORIGIN' -L /lib -L /lib64 -L /usr/lib -L /usr/lib64 -L /usr/local/lib/x86_64-linux-gnu -L /lib/x86_64-linux-gnu -L /usr/lib/x86_64-linux-gnu -L /usr/local/cuda-10.1/targets/x86_64-linux/lib -L /usr/lib/x86_64-linux-gnu/libfakeroot -L /usr/local/lib -L /usr/lib/i686-linux-gnu -L /lib/i686-linux-gnu -L /usr/local/lib/i386-linux-gnu -L /lib/i386-linux-gnu -L /usr/lib/i386-linux-gnu -L /usr/local/lib/i686-linux-gnu -L /home/matija/Dropbox/jai/modules/ --start-group /usr/lib/x86_64-linux-gnu/libpthread.so /usr/lib/x86_64-linux-gnu/libm.so /usr/lib/x86_64-linux-gnu/libc.so /usr/lib/x86_64-linux-gnu/libdl.so /home/matija/Dropbox/jai/modules/stb_sprintf/linux/stb_sprintf.a /usr/lib/x86_64-linux-gnu/librt.so --end-group

Stats for Workspace 3, 'Target Program':
Lexer lines processed: 6153 (8114 including blank lines, comments.)
Front-end time: 0.669836 seconds.
llvm      time: 0.091203 seconds.

Compiler  time: 0.761039 seconds.
Link      time: 0.002718 seconds.
Total     time: 0.763757 seconds.
CloudNC coding standards checking... PASSED
```

And if you were to uncomment some of the code blocks the contain coding standard violations compilation fails (it could be made to just warn instead) and all  errors get reported:

```
matija@chip:~/dev/jai-coding-standards-demo (master)$ jai-linux build.jai 

In workspace #2 ("Target Program"):
/home/matija/dev/jai-coding-standards-demo/main.jai:28,9: Error: 
*** CloudNC coding style violation (Procedure call in return value) ***

    bad_function_call :: () -> bool {
        return truth();

/home/matija/dev/jai-coding-standards-demo/main.jai:31,7: Error: 
*** CloudNC coding style violation (Procedure calls are not allowed in if conditions, assign to a local variable) ***

    }
    if truth() {}

/home/matija/dev/jai-coding-standards-demo/main.jai:32,7: Error: 
*** CloudNC coding style violation (Why are you using more than one negation in an if condition??) ***

    if truth() {}
    if !!true {}

/home/matija/dev/jai-coding-standards-demo/main.jai:33,7: Error: 
*** CloudNC coding style violation (Exceeded maximum binary operator count (4) in if condition) ***

    if truth() {}
    if !!true {}
    if true && true && true && true && true && true {}

/home/matija/dev/jai-coding-standards-demo/main.jai:34,7: Error: 
*** CloudNC coding style violation (Exceeded maximum binary operator count (4) in if condition) ***

    if !!true {}
    if true && true && true && true && true && true {}
    if true && true && true && true && truth() {}

Running linker: /home/matija/Dropbox/jai//bin/lld-linux -flavor Gnu --eh-frame-hdr -export-dynamic -o main /home/matija/dev/jai-coding-standards-demo/.build/main_3_0.obj /home/matija/dev/jai-coding-standards-demo/.build/main_3_1.obj /home/matija/dev/jai-coding-standards-demo/.build/main_3_2.obj /home/matija/dev/jai-coding-standards-demo/.build/main_3_3.obj /usr/lib/x86_64-linux-gnu/crt1.o /usr/lib/x86_64-linux-gnu/crti.o /usr/lib/x86_64-linux-gnu/crtn.o -L /home/matija/dev/jai-coding-standards-demo --dynamic-linker /lib64/ld-linux-x86-64.so.2 -rpath='$ORIGIN' -L /lib -L /lib64 -L /usr/lib -L /usr/lib64 -L /usr/local/lib/x86_64-linux-gnu -L /lib/x86_64-linux-gnu -L /usr/lib/x86_64-linux-gnu -L /usr/local/cuda-10.1/targets/x86_64-linux/lib -L /usr/lib/x86_64-linux-gnu/libfakeroot -L /usr/local/lib -L /usr/lib/i686-linux-gnu -L /lib/i686-linux-gnu -L /usr/local/lib/i386-linux-gnu -L /lib/i386-linux-gnu -L /usr/lib/i386-linux-gnu -L /usr/local/lib/i686-linux-gnu -L /home/matija/Dropbox/jai/modules/ --start-group /usr/lib/x86_64-linux-gnu/libpthread.so /usr/lib/x86_64-linux-gnu/libm.so /usr/lib/x86_64-linux-gnu/libc.so /usr/lib/x86_64-linux-gnu/libdl.so /home/matija/Dropbox/jai/modules/stb_sprintf/linux/stb_sprintf.a /usr/lib/x86_64-linux-gnu/librt.so --end-group

Stats for Workspace 3, 'Target Program':
Lexer lines processed: 6152 (8085 including blank lines, comments.)
Front-end time: 0.679391 seconds.
llvm      time: 0.090365 seconds.

Compiler  time: 0.769756 seconds.
Link      time: 0.002561 seconds.
Total     time: 0.772318 seconds.
CloudNC coding standards checking... FAILED
Error: Compilation terminated.
```