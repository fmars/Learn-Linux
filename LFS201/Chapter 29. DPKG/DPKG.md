Low level package management tools. 
- dpkg: pakcage manager for Debian
- dpkg-query: dpkg database querying


- Debian package has a '.deb' suffix
- `apt-get install` and `apt-get source`
- `dpkg -l`: list all packages
- `dpkg -L`: list all files of a installed package
- `dpkg -W`: info of a installed package
- `dpkg -r`: remove a installed pacakge
- `dpkg-query -W -f '${Installed-Size}\r${Package}\n`: list all installed packages and size
