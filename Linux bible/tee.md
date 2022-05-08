-   Ever wondered, that you want to view the output in real-time and save the output in a file at the same time? Well standard redirection doesn't allow that. No worries, tee command is to the rescue. It reads from stdin and writes to the stdout and files as well.
-   A small handy tool that I use every time with linpeas to read my script results in real time and also saving the output at the machine's /tmp directory.

![](https://i.imgur.com/XoHd92r.png)

-   You can use it with -a flag to don't overwrite an existing file instead just append some more.