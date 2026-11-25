# A Windows checkout can put CRLF into your image

I compared the same benchmark files in two Docker images and every file differed. The content was the same. The difference was line endings.

One image was built on Windows from a checkout with `core.autocrlf=true`. Git turned LF into CRLF on checkout, and `COPY` took the files as they were.

Check:

```sh
head -c 200 file.qasm | cat -A   # lines ending in ^M$ are CRLF
```

Compare while ignoring line endings:

```sh
tr -d '\r' < a.qasm | sha256sum
tr -d '\r' < b.qasm | sha256sum
```

Fix it at the source with a `.gitattributes` in the repo you build from:

```
* text=auto eol=lf
```

Most parsers do not care, but the image hash does. Build the same image on Linux and you get a different hash.
