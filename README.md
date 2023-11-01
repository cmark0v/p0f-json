## P0f(v3)-JSON


What's modified in this fork?
-----------------------------

Love this utility but perplexed by the bizarre array of output formats. 
You get the non-greppable default to stdout which is nice but only to look at. you get the more manageable and grepable but still odd and archaic ``|`` separated file when outputting with ``-o`` and you get the api which lets you query info about specific ip addresses as far as i could tell but wont give you a stream of data to work with. So I hacked this fork to do the default output as json to stdout. It is indeed a hack, implemented by swapping around header defs of the macros used for output, and modifying the code used to do the ``-o file`` output, to instead do json.

so the changes are:

- json output to stdout, contains equivalent information as ``-o file`` output did previously
- ``-o file`` argument defunct
- no other output to stdout
- updated signature database (fingerprints)[https://github.com/mat-1/p0f/blob/main/p0f.fp] 


  - **c. mark0v**

Requirements
------------

- ``lippcap-dev``
- ``build-essential`` not sure what else

Compile
-------

To compile p0f, try running './build.sh'; if that fails, you will be probably
given some tips about the probable cause. 

Run
----

Needs to be run as root, might be able to do it with the correct groups in modern unix OS. it needs access to ``p0f.fp`` at runtime which location can be  configured in ``config.h``

```bash
 ./p0f -i eth0
 ./p0f -i eth0 -d -u p0f-user 
 ./p0f -r some_capture.cap
```

output looks like:

```json
    {"time": "2023/10/28 22:20:54", "mod": "syn", "cli": "192.168.1.101/54922","srv": "172.17.43.133/443", "subj": "cli", "os": "Linux 5.x", "dist": "0", "params": "none", "raw_sig": "4:64+0:0:1460:mss*44,7:mss,sok,ts,nop,ws:df,id+:0"}
```

spits one line datum, each line referencing readings(suck as ``os``) on a host designated by ``subj`` ; or some property of the link layer connecting ``cli`` and ``srv``, or fingerprinting of the protocol used in the connection including server and client details in ``app`` 

see [full readme](doc/README)
