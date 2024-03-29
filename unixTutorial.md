# Linux bash (bourne again shell)

## Using the shell

Baby steps
1) get someone to show you how to open a shell and execute commands
2) get to know the file system and how to navigate through it (see explanatory tutorial above)
3) get to know the basic shell commands described below
4) script away

### Understanding the unix file system:
http://www.december.com/unix/tutor/filesystem.html


## Navigating through the filesystem:

when you first open your shell you will see something like this:

    [chris@troll ~]$

this means, that you have logged into a linux network with username "chris", that you currently use the computer named "troll" and are currently within the space, the filesystem has reserved as your workspace (indicated by "~").

in a more abstract way will always see the information like this:

    [username@computername current_folder]$

You can navigate through the filesystem by using the `cd` (change directory) command right after the `$` sign. Work yourself through the navigation introduction provided with the links above.

### Unix good to knows
- Special characters which are not allowed in file- or foldernames:
`/, .., ~, *, ?, >, <, |, \`

- Linux is case sensitive! "Filename.txt" is not the same as "filename.txt"

- Try to avoid blanks within file- or foldernames, always use underscore or minus, especially, if you create folders or files using the filebrowser!

      e.g. 
      do not use:
      project computational systems biology

      use:
      project_computational_systems_biology

- Use the auto-complete function provided by the tabulator key:

      cd /h

      # hit "tab" once, it should auto-complete your entry to
      cd /home/
      
      # if you hit "tab" twice, it will display the contents of the directory, without disrupting your command entry
      cd /home/

      admin/ apps/  conf/  edu/   proj/  user/

      cd /home/

      # if there are more than one files or directories with the same starting letters (e.g. /home/ or /hello/) you will have to enter the next letter, before autocomplete will work
      
      cd /h
      # "tab" will  not work

      # but:
      cd /he

      # "tab" returns:
      cd /hello/

- get help! either by using the `man` (manual) command or by using `--help` commandline option:

      man [command]
      # example
      man cp

      [command] --help
      # example
      cp --help

- most of the command line programs displaying the contents of text files can be ended by simply pressing "q".

- how to stop a running program (process will be on hold in the background):

      ctrl + z

- how to kill a running program (process will be terminated):

      ctrl + d

- There is no paper basket on the command line! If you remove files or directories using the shell, there is no easy file recovery!


## Basic Bash commands

- `ls` ... display files and directories within your current directory
- `ls -l` ... like ls, but display additional information
- `ls -a` ... like ls, but also display hidden files and directories
- `ls -l -a` ... combination of `ls -l` and `ls -a`

`- ls ../[foldername]` ... display the contents of directory "foldername" residing on the same hierarchical layer as the folder we are currently residing in

- `ll` ... like `ls -l`

- `pwd` ... print working directory, prints the complete path from root until the directory we are currently residing in

      [chris@troll papers]$pwd
      /home/users/chris/papers/
      [chris@troll papers]$

- `cd [path]` ... change directory; move within the filesystem from your current directory to another directory specified in your [path]

      [chris@troll ~]$cd /home/user/franz/

- `cd ..`   ... change from the current directory to the directory directly above.

- `mkdir [foldername]` ... create folder "foldername" at your current position within the filesystem

      [chris@troll papers]$mkdir cell_papers

- `rmdir [foldername]` ... remove folder "foldername" from the filesystem. will only work, if the folder is empty.

      [chris@troll papers]$rmdir cell_papers

- `rm [filename]` ... remove file "filename" from the filesystem.

      [chris@troll papers]$rm 2011_Science_2282772.pdf

- `rm [foldername] -r` ... remove folder "foldername" from the filesystem including all files and folders it contains.

      [chris@troll work]$rm papers -r

- `cp [path1/source] [path2/target]` ... copy file "source" residing at location "path1" to file "target" residing at location "path2"

      [chris@troll work]$cp /home/user/chris/work/papers/2011_Science_2282772.pdf /home/users/chris/work/papers/science/2011_Science_2282772.pdf

- `cp path1/source ./target` ... copy file "source" residing at location "path1" to file "target" at the current location

      [chris@troll science]$cp /home/user/chris/work/papers/2011_Science_2282772.pdf ./2011_Science_2282772.pdf
      # will copy the file 2011_Science_2282772.pdf
      # from location /home/user/chris/work/papers/
      # to location /home/users/chris/work/papers/science/

- `cp path1/source` . ... copy file "source" residing at location "path1" to the current location, keeping the same filename.

      [chris@troll science]$cp /home/user/chris/work/papers/2011_Science_2282772.pdf .
      # will copy the file 2011_Science_2282772.pdf
      #  from location /home/user/chris/work/papers/
      #  to location /home/users/chris/work/papers/science/

- `mv [path origin]/[filename] [path target]` ... same as "cp" command, but moves the file from one location to the other, deleting the original file.

- `echo text` ... prints "text" onto the screen

      [chris@troll work]$echo hurray for icecream!
        
- `echo text > filename.txt` ... saves "text" into file "filename.txt" which will be created at the current filesystem location

      [chris@troll work]$echo hurray for icecream! > important.txt

- `cat [filename]` ... prints the contents of file "filename" onto the screen.

      [chris@troll work]$cat important.txt

- `less [filename]` ... displays the contents of file "filename" in the screen, contents are scrollable by using "up" and "down" keys. end this by pressing "q"

      [chris@troll work]$less important.txt

- `history` ... list of all commands executed within this terminal

      [chris@troll ~]$history

- `exit` ... close the shell

      [chris@troll ~]$exit


## Further Basic Bash command line options:

- `[command] > [filename]` ... will write the output of a specific [command] to a specific file. note, that an already existing file with the same filename at the same location will be replaced!

      [chris@troll work]$echo hurray for icecream! > important.txt
      [chris@troll work]$less important.txt
      [chris@troll work]$echo another hurray for schnitzel! > important.txt
      [chris@troll work]$less important.txt

- `[command] >> [filename]` ... will append the output of a specific [command] to a specific file. If the file does not exist yet, it will be created.

      [chris@troll work]$echo chicken >> shopping_list.txt
      [chris@troll work]$less shopping_list.txt
      [chris@troll work]$echo onions >> shopping_list.txt
      [chris@troll work]$echo lemons >> shopping_list.txt
      [chris@troll work]$echo olive oil >> shopping_list.txt
      [chris@troll work]$echo rosemary >> shopping_list.txt
      [chris@troll work]$less shopping_list.txt

- `[command] &` ... start a program running in the background, getting back the shell.

      [chris@troll work]$firefox &

- `[command]`       ... start a program

      [chris@troll work]$firefox

- `ctr + z`         ... stop the execution of the program
- `bg`              ... restart the program, but keep it running in the background (bg)

      [chris@troll work]$bg

- `fg`              ... if you have a program running in the background, get it back by fg (foreground)

      [chris@troll work]$fg

- `command_1 | command_2` ... `|` ... "pipe". executes "command_1", directs the output of this command not to the screen, but the second command "command_2". Only then the output of "command_2" will be printed onto the screen.


## Further useful Bash Commands:

- `ssh [computername]`        ... connect to another computer using secure shell (an encrypted connection to another computer within the network). once connected, the cpu of this computer will be used for all further actions. makes sense to do large calculations on the server rather than on the local machine. Might require a password.

      [chris@troll work]$ssh server4
      [chris@server4 ~]$

      notice that the computername in the bash shell has changed from "troll" to "server4"

      use the command "exit" to close the ssh connection to another computer.
      [chris@server4 ~]$exit
      [chris@troll ~]$

- `scp [computername]:[remote_directory]/[filename] [current_directory]`   ... copy file "filename" from a remote computer or network into the "current_directory" using encrypted file copy

      [chris@troll work]scp server4:/temp/work/* /home/user/chris/work/

- `wc [filename]`      ... counts lines, words and letters within file "filename"

      [chris@troll work]wc shopping_list.txt


More nice bash commands, use the --help option for in depth information:

- `top`     ... displays all processes running on the currently used computer. exit using "q"

- `gzip`    ... compress/uncompress files

- `tar`     ... compress/uncompress files


## Command line programs for text file handling

Use the --help option for in depth information, links provided or the interwebs.

- `sort`    ... Sorts standard input then outputs the sorted result on standard output.
- `uniq`    ... Given a sorted stream of data from standard input, it removes duplicate lines of data (i.e., it makes sure that every line is unique).
- `fmt`     ...Reads text from standard input, then outputs formatted text on standard output.
- `pr`      ...Takes text input from standard input and splits the data into pages with page breaks, headers and footers in preparation for printing.
- `head`    ... Outputs the first few lines of its input. Useful for getting the header of a file.
- `tail`    ... Outputs the last few lines of its input. Useful for things like getting the most recent entries from a log file.
- `tr`      ... Translates characters. Can be used to perform tasks such as upper/lowercase conversions or changing line termination characters from one type to another (for example, converting DOS text files into Unix style text files).
- `grep`    ... Examines each line of data it receives from standard input and outputs every line that contains a specified pattern of characters.
                https://www.panix.com/~elflord/unix/grep.html
- `sed`     ... Stream editor. Can perform more sophisticated text translations than `tr`.
- `awk`     ... An entire programming language designed for constructing filters. Extremely powerful.
