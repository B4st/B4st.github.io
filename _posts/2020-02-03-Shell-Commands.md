---
title: "Shell Commands"
date: 2020-02-03T15:34:30-04:00
categories:
  - UNIX
tags:
  - Commands
---

These lists will be all over the internet, just lists of commands and what they do. This is just a comprehensive list of the ones that I've used or found useful, organized in a manner that works for me.

`--help` gives information on a command

## Directory Commands

- **cd** = change directory
  - `cd directory` will move you to a subdirectory
  - use " " around the name if it has spaces
  - use forward slashes when moving through multiple subdirectories at a time or changing to a new directory. If you are in /usr `cd bin` (without a /) will put you in the subdirectory /usr/bin, while `cd /bin` puts you in /bin a folder outside of /usr
  - `cd .` will bring you to the directory you are currently in
  - `cd ..` will move you up one directory for ex. if you're in /usr/bin/tmp, `cd ..` moves you to /usr/bin. You can move multiple levels by adding forward slashes ex. `cd ../..` moves you to /usr
  - `cd -` will bring you back to the previous directory

- **ls** = list
  - lists the files in the current directory, has various options to control the specific output
  - `ls -l` long format, displaying Unix file types, permissions, number of hard links, owner, group, size, last-modified date and filename
  - `ls -a` lists all files in the given directory, including those whose names start with "." which are otherwise hidden
  - `ls -R` recursively lists subdirectories, for ex. `ls -R /` would list all files

- **mkdir or md** = make directory
  - `mkdir name_of_directory` will create a new sub directory of the current one with the specified name
  - `-p` the path option, will also create all directories leading up to the desired directory that do not exist already. For ex. `mkdir -p a/b` will create directory a if it doesn't exist, then will create subdirectory b
  - `-m` the mode option, allows you to specify the octal permissions of directories created by mkdir.

## File Manipulation

- output redirection
  - Most commands will give their output to the terminal but you can redirect them to a file instead
  - `> newfile` = create new file or replace file contents with new output
  - `>> existingfile` = add output of file to existing file

- **cat** = concatenate
  - reads files sequentially, writing them to standard output, the name comes from its function to concatenate files.
  - `cat filename.extension` will print the contents of the file to the terminal, you always use the file extension
  - `cat file1 file2` will print the concatenation of the two files to the terminal
  - `cat > newfile` will create a new empty file of the name

- **grep** = globally search a regular expression and print
  - Given one or more patterns, grep searches input files for matches to the patterns. When it finds a match in a line, it copies the line to standard output (by default), or produces whatever other sort of output you have requested with options.
  - `grep [options] [patterns] [file]`

- **wc** = word count
  -  reads either standard input or a list of files and generates one or more of the following statistics: newline count, word count, and byte count
  - `wc -l <filename>` prints the line count (note that if the last line does not have \n, it will not be counted)
  - `wc -c <filename>` prints the byte count
  - `wc -m <filename>` prints the character count
  - `wc -w <filename>` prints the word count

### CSV Manipulation

  - **head** = print the header
    - `head filename` will print the first 10 entries of the file to the terminal
    - `head -n 5 filename` option n lets you change the number of entries printed, in this case to 5

- **cut**
  - `cut -d , -f 2,4-6 input.csv`
  - `-d` tells cut that columns are separated by commas
  - `-f 2,4-6` tells it to extract column 2 and columns 4-6. The -f argument can take a single column number or a comma-separated list of numbers and ranges.
