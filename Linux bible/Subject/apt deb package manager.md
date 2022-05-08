deb is a package that contain application and all of its associated file and metafile(documentation, config, dependencies, and other information).

Some application that works with deb packaging:
- Ubuntu Software center: Open with Activity -> Software
- aptitude: command-line application that provide screen-oriented menu.
- apt: The main way to work with deb package, usually used more for automatic lookups from the repository.
- dpkg: Tools to work with a debian package
![[apt various command.png|700]]
## dpkg
```bash
dpkg -L package #list all file installed in the package
dpkg -s package #display the package meta-data
```
Usually package documentation is located in the `/usr/share/doc` folder for each package.