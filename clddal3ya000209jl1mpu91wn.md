# How to Easily Spell-Check all the Files in a Folder and Display any Misspelled Word Found, Along With the Filename in Linux Terminal.

Someone once said "To make mistakes is human but to edit is divine"

Typographical errors (misspelling words) are part of our daily lives. We make typos when texting or writing in plain text, and somethings editing (correcting) our errors can be a hassle especially when we have a lot of files to edit.

In this article, I'm going to show you how you can easily spell-check multiple files efficiently with a bash script in a Linux terminal.

## Let's get started.

For this exercise, we will be using GNU Aspell. The Aspell command is used as a spell checker in Linux. Generally, it will scan the given files or anything from standard input then it checks for misspellings. Finally, it allows the user to correct the words interactively.

First, you have to install Aspell on your Linux system. Simply open your terminal and type in the following command and press enter to install Aspell on your system:

`sudo apt install aspell`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674742856342/e09a8222-7e6d-47ca-9c34-b5d871bc0897.png align="center")

After installation, type in the following command to verify you have Aspell installed in your system:

`which aspell`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674743439546/3fc21dfa-fc79-4873-aadd-6031133fb84d.png align="center")

You should get `/usr/bin/aspell` as your output.

The traditional way to spell-check with Aspell would be to run it against a file as shown below.

`aspell -c <filename>`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674744796535/c83c66ca-47af-490b-b7c8-178ca4d53b91.png align="center")

In the image above I spell-checked the paper.txt file and was greeted with Aspell interactive shell that makes file editing simple.

> ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674744857748/ddf617d8-3639-40bd-a722-07fc8aabeb85.png align="center")

The top pane shows the file with any errors (or perceived errors) highlighted. The bottom lists the suggested corrections (based on Aspell's default dictionary) and various commands that you can use.

In the screen capture above, Aspell flagged the acronym "moni" as an error and suggested several alternatives. I can do the following:

* Press the number on my keyboard next to an alternative to replace the misspelled word with another one.
    
* Press i to ignore that instance of the perceived error or press I to ignore all instances of the error.
    
* Press a to add the word to Aspell's dictionary.
    
* Press r or R to replace that instance or all instances of the word with a new word.
    
* Press X to exit.
    
    This is a great way to spell-check a file in your terminal, but what if you have multiple files in a folder and you have to spell-check each one of them, that could take a lot of time, don't you agree?
    

Here's a bash script that will help you automate spell-checking multiple files in a folder.

```bash
! /bin/bash



#This is a script that ask user for a folder/

#print each filename in the folder and then a sorted list of misspelled words together with the number of times they occur/

#then help the user correct the listed misspelled words in each file.



echo "Please enter a folder"



read folder



cd $folder



echo 'Sorting misspelled words..'



for f in *.txt ; do echo $f ; aspell list < $f | sort | uniq -c ; done



echo "Please correct misspelled words..."



for f in *.txt; do aspell check $f; done



echo "Spell checking complete."

```

The bash script asks users for a folder, prints each file name in the folder and then a sorted list of misspelled words together with the number of times they occur then helps the user correct the listed misspelled words in each file.

All you have to do is copy the script above and paste it into a file with the command:

`nano file_spell_checker.sh`

Note that the file extension is .sh

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674747701608/52f9f8aa-f686-4a03-b1dd-0c036a5d0b66.png align="center")

Press ctrl O to write out, hint enter and ctrl X to save the file.

To run the bash script, enter the command on your terminal

`sh file_spell_checker.sh`

The script will ask you for a folder or directory name like `Documents/` , scan through each file in the folder, and help you spell-check each of them efficiently and fast.

---

That's how you can automate file spell-checks with a bash script. If you find it useful please share this article.