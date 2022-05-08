rpm is like deb, but is mainly used by Red Hat/Fedora and its fork.
Some command that works with rpm:
- `rpm`: The first tool to manage rpm
- `yum`: newer than rpm
- `dbf`: the newest tool

### Installing
```bash
#install
rpm -i zsh-5.5.1-6.e18.x86_64.rpm
#update, with options usually used
rpm -Uhv zsh-5.5.1-6.e18.x86_64.rpm
# only update already installed package
rpm -Fhv *.rpm

rpm -Uhv --replacepkgs emacs-26.1-5.el8.x86_64.rpm

rpm -Uhv --oldpackage zsh-5.0.2-25.el7_3.1.x86_64.rpm
#remove package
rpm -e emacs
```
- `-i` install the package(must be full name)
- `-U` update if the previous version of the package is installed.
- `-h` print hashes signs.
- `-v` more verbose output.
- `-F` Freshen, install(update) only if previous version of the package is already installed.
- `--replacepckgs` reinstall packages(in case some componenet are missing)
- `--oldpackage` replace newer package with earlier version
- `-e` remove selected package, but leave behind dependencies. Wouldn't work if the package is another package's dependencies.

### Querying
```bash
rpm -qi zsh
rpm -ql zsh
rpm -qd zsh
rpm -qip zsh-5.7.1-1.fc30.x86_64.rpm
```
- `-q` information about the package
- `-qi` include description
- `-ql` list of files
- `-qd` documentation
- `-qc` configuration files
- `--requires` list what the packages need to be installed
- `--provides` what version of software a package provides
- `--scripts` what scripts are run before & after it's installed/removed.
- `--changelog` what's changed
- `-p` used to query package in your local directory(instead of the databases)

### Verifying
```bash
rpm -V zsh

missing c /etc/zshrc 
S.5....T. /bin/zsh
```
`-V` verify the rpm package. It show what has been changed since the package first installed. The second output `S.5....T. /bin/zsh` show what the changes are. Dot(`.`) will show numbers/letter if this type of change is made:
![[rpm verify change.png| 400]]

Use `rpm -i --replacepkgs zsh-5.7.1-1.fc30.x86_64.rpm` to restore the packages. No output means that it's the same as the original state.
