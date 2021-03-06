title: "Finding leaks on Solaris (w/o Valgrind)"
date: 2010-10-09 20:04:18
categories:
- Damaged Bits
tags:
- solaris
- illumos
---

Premise: I write a lot of C code.  I run a lot of Solaris.

Sadness: One of my favorite tools ever made is Valgrind.  Valgrind does not run on Solaris.

A lot of the C code I write is event-driven and as such (complicated) it is harder to write code and leaked memory is a common residual of this more complicated coding effort.  Memory leaks suck.  Most of the code I write is systems-level code and as such is exercised heavily and needs to run for months or years without restart.  So, leaks more than suck.

Valgrind will tell you exactly where you're leaking (and it feels like it tells you why)... BFM.

On Solaris, we have a different option: libumem.

I compile almost all of my apps against libumem, which has the effect of replacing malloc/free/and friends with a more multi-processor scalable slab-allocator implementation than the default one in libc.  A very useful feature in libumem is debugging and leak detection... how?

First, let's assume you've compiled your app with debugging symbols (-g in the compiler flags).  Next, I'll assume you didn't have the foresight to link against libumem (-lumem on the linker line).  We need to link in libumem and we need to turn on debugging.  In the case of this example, our app is the Apache web server `httpd`.

``` bash
LD_PRELOAD=libumem.so.1 UMEM_DEBUG=default /opt/apache22/bin/httpd
```

Now, just find the process ID of exampled (perhaps as easy as a `pgrep exampled`). It is leaking, so we'll use the process ID 666. To find the leaks (without killing or restarting), we can simply use the mdb umem helper ::findleaks on the running process.

``` bash
echo "::findleaks -d " | mdb -p 666 > leak-profile.txt
```

Note you can also do this from a core... very nice.

The output has a summary and then individual profile records grouped by stack trace of allocation, like:

``` mdb mdb output
umem_alloc_16384 leak: 203 buffers, 16384 bytes each, 3325952 bytes total
             ADDR          BUFADDR        TIMESTAMP           THREAD
                             CACHE          LASTLOG         CONTENTS
          1d36b60          1d37000   70235c07811a50              197

                  libumem.so.1`umem_cache_alloc_debug+0x152
                  libumem.so.1`umem_cache_alloc+0x1a2
                  libumem.so.1`umem_alloc+0xdb
                  libumem.so.1`malloc+0x59
                  libapr.so.0.2.12`allocator_alloc+0x466
                  libapr.so.0.2.12`apr_palloc+0xda
                  bio_filter_out_ctx_new+0x37
                  ssl_io_filter_init+0xb1
                  ssl_init_ssl_connection+0x2cd
                  ssl_hook_pre_connection+0x123
                  ap_run_pre_connection+0xc3
                  ap_process_connection+0x30
                  process_socket+0xa8
                  worker_thread+0x2d8
                  libapr.so.0.2.12`dummy_worker+0x30
```

mod_ssl... Why are you so mean to me?
