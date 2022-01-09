graudit
===============================================================================
graudit is a simple script and signature sets that allows you to find potential 
security flaws in source code using the GNU utility grep. It's comparable to 
other static analysis applications like RATS, SWAAT and flaw-finder while 
keeping the technical requirements to a minimum and being very flexible.

Installation
===============================================================================
Installation can be done as a user or globally as root, simply run make with
the userinstall or install argument. It is however recommended to use graudit
directly by cloning the git repository as it includes additional database rules
not included on the distribution files. This also enables you to get updates
between releases. To do this run the following command:

```
git clone https://github.com/wireghoul/graudit
```

You can then symlink graudit so it is in path:
```
ln -s path/to/graudit/graudit ~/bin/graudit
```

If graudit is not in your home directory you may wish to set the GRDIR
environment variable to the signatures directory in your `.bashrc` file.
```
export GRDIR=/path/to/graudit/signatures
```

Usage
===============================================================================
graudit supports several options and tries to follow good shell practices. For
a list of the options you can run graudit -h or see below. The simplest way to 
use graudit is;

```
graudit [opts] /path/to/scan

OPTIONS
  -d <dbname> database to use or /path/to/file.db (uses default if not specified)
  -A scan unwanted and difficult (ALL) files
  -x exclude these files (comma separated list: -x *.js,*.sql)
  -i case in-sensitive scan
  -c <num> number of lines of context to display before and after a match, default is 1

  -B suppress banner
  -L vim friendly lines
  -b colour blind friendly template
  -z suppress colors
  -Z high contrast colors
  
  -l lists databases available
  -v prints version number
  -h prints this help screen
```

Databases
===============================================================================
graudit uses extended regular expressions (POSIX) as it's signatures and comes 
with several databases ready for use. You can extend the existing databases or 
make your own if you require additional signatures.

Databases can be loaded from multiple locations, the order of precedence is as
follows:
  1. Custom location specified via the GRDIR environment variable
  2. /usr/share/graudit/
  3. $HOME/.graudit/
  4. A relative signatures/ directory from the graudit location
  5. A relative misc/ directory from the graudit location
  6. $HOME/graudit/signatures/
  7. Any file that is specified with a full path, i.e: /home/user/my.db
  8. Rules can be read from stdin by supplying - or /dev/stdin as the database

A list of the database files in order of precedence is shown with the -l switch:
`graudit -l`

The following databases are included:
  - actionscript
  - android
  - asp
  - c
  - cobol
  - default (used if -d argument is omitted)
  - dotnet
  - exec
  - fruit
  - go
  - ios
  - java
  - js
  - perl
  - php
  - python
  - nim
  - ruby
  - secrets
  - spsqli
  - sql
  - strings
  - xss

Troubleshooting:
===============================================================================
- 'Operation Not Permitted'. See if your Terminal has Full-Disk Access.
- 'Errors with Tests'. If the tests do not work on your machine, identify which tests are failing and comment out the testing lines. You'll lose test coverage, but graudit should function then. 
- 'command not found: graudit'. Check to see if you've added graudit to your $PATH variable. Open a new terminal and test that graudit works.
- 'no matches found:'. This happens if you're using a non-bash shell most likely. Your arguments should be quoted or escaped.

Tutorial
===============================================================================
Here are some examples for testing source code.

```
graudit . \\ Scan everything with default rules. The default rules do not mean "Every Rule" but are a selection of rules from each DB. You should be selective about which DB you should use.

graudit -d xss . \\ Scan everything with the included xss database of rules.

graudit -d customRules . \\ Scan everything with your own custom database of rules.

graudit ~\Work\Repos\VulnerableApp \\ Scan a directory you're not currently in.

graudit -B . \\ Scan everything with no banner. Good for piping.

graudit -x *.min.js . \\ Exclude minified JavaScript, which is difficult to audit.

graudit -x '*.min.js' . \\ Quoting or Escaping arguments helps deal with globbing issues across shells.
graudit -x \*.min.js .

graudit -i . \\ Case insensitivity is good for identifying secrets, but otherwise may lead to higher false positives.

graudit -A . \\ Images, compressed, and binary files are typically excluded by default. Use -A to scan everything at the expense of performance.

graudit -l \\ Will list the databases currently being used in graudit and their locations.

```


Contributing
===============================================================================
If you would like to contribute to graudit, please fork the repository at 
https://github.com/wireghoul/graudit and use that. In particular language rules,
additional scripts and documentation contributions are very welcome.
If you like graudit then please say thanks and share it around.

If you wish to get in contact with me, shoot me a line on github or twitter: 
@wireghoul

Credits
===============================================================================
  * Wireghoul - http://www.justanotherhacker.com
  * Various others - see Changelog
