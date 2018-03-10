---
title: "Encode Base36 in Python"
date: 2015-02-01T15:11:18+01:00
tags: [ "python" ]
---
The cleanest way I've found so far to encode/decode between base36 and integers in Python. The primary purpose of this was to write a url shortener based on looking up a numbered url from a database.
<!--more-->

    #!/usr/bin/env python

    import string

    alphabet = string.digits + string.ascii_lowercase

    def enc(n, base=36):
        out = []
        while n > 0:
            n, r = divmod(n, base)
            out.append(alphabet[r])
        return(''.join(reversed(out)))

    def dec(s, base=36):
        return int(s, base)
