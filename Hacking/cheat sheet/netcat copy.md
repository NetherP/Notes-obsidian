For copying file
On the receiving end running,

```bash
nc -lvnp 1234 > out.file
```

will begin listening on port 1234.

On the sending end running,

```bash
nc -w 3 [destination] 1234 < out.file
```
`-w` is connection/timeout timer(3 seconds)