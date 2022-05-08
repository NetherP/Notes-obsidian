rpm tools,to install rpm package/application. It became a symbolic link to `dnf` as dnf is added to become the default RPM package manager for RHEL 8. Almost all `yum` command can be substituted with `dnf`.
```bash
yum [options] command
yum install firefox
```
The YUM facility finds the latest version of that package in the repo, download, and install it.

`/etc/yum.conf` is the main yum configuration file, while `/etc/yum.repos.d` is where the location of your repos is stored. `man yum.conf` to see more details.

### Search
```bash
yum search keyword
yum search editor
```
search packages containing *keyword* in the name or description of the package.

```bash
yum info package_name
yum info emacs
```
query the *package_name* info about that package.

```bash
yum provides keyword
yum provides dvdrecord
```
query what package *keyword* come from. *Keyword* can be a command, config files, or library name.

```bash
yum list emacs
yum list available
yum list installed
yum list all
```
The `yum list` command list the package based on the argument. If the argument is a package name(as in the first example) it will show the version and it's repository. `available` show all available package. `installed` show all installed package and `all` show both.

```bash
yum deplist emacs
```
`deplist` show what components that package is dependent on, and the package that components comes in.

### Installing and Removing
```bash
yum install emacs
```
Install packages and all it's dependencies

#### options
- `-y` to automatically confirm installation.

```bash
yum reinstall zsh
```
reinstall the package, in case some component is missing(deleted accidentally).

```bash
yum remove emacs
```
remove the package and all its unused dependency.

```bash
yum history
yum history info 12
yum history undo 12
```
`history` show your yum activities.
`info` show what package involved in that transaction number. `undo` remove all package in that transaction.

### Updating
```bash
yum check-update
yum update
yum update cups
```
`check-update` check for available package update. `update` updates all available packages. The third command `yum update package_name` update only that package.

```bash
yum grouplist
yum groupinfo LXDE
yum groupinstall LXDE
yum groupremove LXDE
```
`grouplist`show list of package group. `groupinfo` show information for that package group, show mandatory(must be included), default(can be excluded), optional(can be included). `groupinstall` install/update all package in that group. `groupremove` removes package in that group.

### Maintaining
```bash
yum clean packages
yum clean metadata
yum clean all
```
clean the cache

```bash
yum check
rpm --rebuilddb
```
`check` the RPM database, in case its corrupted. The second command try to repair the database.

### Downloading RPM
```bash
yumdownloader package_name
yumdownloader firefox

dnf download firefox
```
download the rpm package at the current directory.

