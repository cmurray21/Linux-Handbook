# Tar Command in Linux

What is tar? Tar stands for "tape archive" and refers to a practice from the earlier days of computing when data was backed up to tapes. Despite the nostalgiac origin of the name, tar is very powerful and uses modern technologies to archive and compress files.

The tar command is important for linux users to understand. Before we get too deep into the subject, let's start things off with a little clarification. 

* Archiving - The act of storing multiple files as one or more files.
* Compression - The act of shrinking a larger file or files.

I'm a little embarassed to admit, I didn't know there was a difference for a long time. The two concepts are closely related. This isn't the only confusing thing about tar. More about that later. Let's discuss some other tools to give you some background.

## Common Archive and Compression Tools
Most of us come to Linux after years of using Windows or Mac. If that's the case, then when you think of compressed files, you probably think of '.zip' or '.rar' files. 

Actually, there are a lot of different types of compression. Tar and g-zip (tar.gz) dominate Linux systems and both are accessible via the tar command. 

It's important to remember that extensions are not necessary on Unix-based systems. A unix-based operating system can typically identify files by their headers regardless of extension, but using the common naming scheme can help avoid confusion. I recommend following convention, especially if you will be sharing the files with other users.

Let's take a look at common conventions to provide some clarity. 

# Naming Conventions for Common Archive Types
## .Tar 
This is a tarball file. It is only an archive and no compression is performed.

## .Tar.Gz or .tgz
This is the extension of an archive that has been compressed with G-Zip. 

## .Tar.Bz2 or .tbz
This is the extension of an archive that has been compressed with Bz2. This is a relatively new technology. It features a higher ratio of compression, but that increased shrinking power means it takes a bit longer to complete.  

## .Tar.xz or .txz, etc.
Tar also features built in support for xz, lzip, and more. These tools primarily use the same compression algorithim, LZMA. The popular 7z that has become fairly common in Windows environments also uses this algorithim. Further differences in the files result from structure and metadata. I won't go into these details in our examples, but I wanted to mention them.


# Documents to be Archived

For this article, I want to demonstrate some of the common methods for archiving and compressing files using tar.

I have assembled some files of different types in my Documents folder. There are stock images and some text files. We will look at how the file size is changed by the compression through several examples.

Here is a complete list of the documents that will be used. 

```
christopher@linux-handbook:~/Documents$ ll
total 7404
drwxr-xr-x  2 christopher christopher    4096 May  5 10:55 ./
drwxr-xr-x 17 christopher christopher    4096 May  5 10:59 ../
-rw-r--r--  1 christopher christopher 5094932 Apr 30 23:54 aerial-view-of-bushes-on-sand-field-3876435.jpg
-rw-r--r--  1 christopher christopher   13268 Apr 30 23:57 lorem1.txt
-rw-r--r--  1 christopher christopher   13268 Apr 30 23:57 lorem2.txt
-rw-r--r--  1 christopher christopher   13268 Apr 30 23:57 lorem3.txt
-rw-r--r--  1 christopher christopher   13268 Apr 30 23:56 lorem.txt
-rw-r--r--  1 christopher christopher 2411919 Apr 30 23:54 mountain-range-2397645.jpg

```

I've already touched on the capabilites of tar. It is a powerful tool with a lot of options. The various options and file types may make the command appear more complicated than it really is. As always, my goal is to demystify the command line. So let's break things down into smaller pieces before combining several options in the examples. 

Here is a table of commonly used options. Keep in mind, this is just the beginning. I reccomend going throught the help documentation yourself to find even more possibilities once you feel comfortable.

Switch | Expansive Options | Description |
|--|--|--|
-c, |--create              | create a new archive
-d,| --diff, --compare     | find differences between archive and file system
-r,| --append               | append files to the end of an archive
-t,| --list                 | list the contents of an archive
-u,| --update               | only append files newer than copy in archive
-x,| --extract, --get      | extract files from an archive                       
-j,| --bzip2                | filter the archive through bzip2
-z,| --gzip, --gunzip, --ungzip   | filter the archive through gzip

# Examples
## 1) Create a Tarball
Earlier I mentioned the common file types associated with the tar command. This is perhaps the most basic. Compression will not be applied so the file will occupy at least as much space as the files in the documents folder.

### Input: 
```
christopher@linux-handbook:~$ tar cvf doc.tar ~/Documents
```
tar cvf (create, verbose, file-archive)

### Results:
```
christopher@linux-handbook:~$ ll doc.tar
-rw-r--r-- 1 christopher christopher 7567360 May  5 11:00 doc.tar
```

#### Size: 7567360

## 2) Create a G-Zip Tarball
The next file we will attempt is a G-Zip Tarball.

### Input: 
```
christopher@linux-handbook:~$ tar cvfz doc.tar.gz ~/Documents
```
tar cvfz (create, verbose, file-archive, g-zip)

### Results:
```
christopher@linux-handbook:~$ ll doc.tar.gz 
-rw-r--r-- 1 christopher christopher 7512498 May  5 11:01 doc.tar.gz
```
#### Size: 7512498

## 3) Create a Bz2 Tarball
The next file we will attempt is a Bz2 Tarball.

### Input: 
```
christopher@linux-handbook:~$ tar cvfj doc.tar.bz2 ~/Documents
```
tar cvfj (create, verbose, file-archive, bz2 type)

### Results:
```
christopher@linux-handbook:~$ ll doc.tar.bz2
-rw-r--r-- 1 christopher christopher 7479782 May  5 11:04 doc.tar.bz2
```
#### Size: 7479782

## 4) List Archive Contents

We can use the `-t` option to view the contents of an archive file. This works the same whether the file is compressed or not. It will list the actual file size, not the compressed size. 

```
christopher@linux-handbook:~$ tar -tvf doc.tar
drwxr-xr-x christopher/christopher 0 2020-05-05 10:55 home/christopher/Documents/
-rw-r--r-- christopher/christopher 5094932 2020-04-30 23:54 home/christopher/Documents/aerial-view-of-bushes-on-sand-field-3876435.jpg
-rw-r--r-- christopher/christopher   13268 2020-04-30 23:57 home/christopher/Documents/lorem1.txt
-rw-r--r-- christopher/christopher   13268 2020-04-30 23:56 home/christopher/Documents/lorem.txt
-rw-r--r-- christopher/christopher 2411919 2020-04-30 23:54 home/christopher/Documents/mountain-range-2397645.jpg
-rw-r--r-- christopher/christopher   13268 2020-04-30 23:57 home/christopher/Documents/lorem3.txt
-rw-r--r-- christopher/christopher   13268 2020-04-30 23:57 home/christopher/Documents/lorem2.txt
```

## 5) Append Files 
We can append files to a tarball archive using `-r`. You cannot add files to a compressed archive without extracting them first using the tar command. You can also append using the `-u` option for update. This option is supposed to only add the new files according to help docs, but in my practice, it worked the same as append, adding new copies of all the files.

```
christopher@linux-handbook:~$ tar -rvf doc.tar ~/Documents/
```
tar rvf (update, verbose, file-archive)

```
christopher@linux-handbook:~$ tar -tvf doc.tar
drwxr-xr-x christopher/christopher 0 2020-05-05 10:55 home/christopher/Documents/
-rw-r--r-- christopher/christopher 5094932 2020-04-30 23:54 home/christopher/Documents/aerial-view-of-bushes-on-sand-field-3876435.jpg
-rw-r--r-- christopher/christopher   13268 2020-04-30 23:57 home/christopher/Documents/lorem1.txt
-rw-r--r-- christopher/christopher   13268 2020-04-30 23:56 home/christopher/Documents/lorem.txt
-rw-r--r-- christopher/christopher 2411919 2020-04-30 23:54 home/christopher/Documents/mountain-range-2397645.jpg
-rw-r--r-- christopher/christopher   13268 2020-04-30 23:57 home/christopher/Documents/lorem3.txt
-rw-r--r-- christopher/christopher   13268 2020-04-30 23:57 home/christopher/Documents/lorem2.txt
drwxr-xr-x christopher/christopher       0 2020-05-05 11:11 home/christopher/Documents/
-rw-r--r-- christopher/christopher 5094932 2020-04-30 23:54 home/christopher/Documents/aerial-view-of-bushes-on-sand-field-3876435.jpg
-rw-r--r-- christopher/christopher   13268 2020-04-30 23:57 home/christopher/Documents/lorem1.txt
-rw-r--r-- christopher/christopher   13268 2020-04-30 23:57 home/christopher/Documents/lorem4.txt
-rw-r--r-- christopher/christopher   13268 2020-04-30 23:56 home/christopher/Documents/lorem.txt
-rw-r--r-- christopher/christopher 2411919 2020-04-30 23:54 home/christopher/Documents/mountain-range-2397645.jpg
-rw-r--r-- christopher/christopher   13268 2020-04-30 23:57 home/christopher/Documents/lorem3.txt
-rw-r--r-- christopher/christopher   13268 2020-04-30 23:57 home/christopher/Documents/lorem2.txt
```

## 6) Extract Files 
Now that we have seen how the different types of compression affect the overall file size, let's look at extracting those files. 

```
christopher@linux-handbook:~$ cd docs
christopher@linux-handbook:~/docs$ tar -xvf ~/doc.tar.gz 
```

I changed into  a new directory called docs. Then I used tar xvf (extract, verbose, file-archive) to unpack the contents here. 

```
christopher@linux-handbook:~/docs/home/christopher/Documents$ ls
aerial-view-of-bushes-on-sand-field-3876435.jpg  lorem1.txt  lorem2.txt  lorem3.txt  lorem.txt  mountain-range-2397645.jpg
```
It's important to note that tar retains file structure so when I extract the files, the are in /home/christopher/Documents. To avoid this, you can switch to the desired directory (~/Documents) and copy all files using the * wildcard instead of the directory structure.

# Conclusion
Did you enjoy our guide to the `tar` command? I hope all of these tips taught you something new. If you like this guide, please share it on social media. If you have any comments or questions, leave them below. If you have any suggestions for topics you'd like to see covered, feel free to leave those as well. Thanks for reading. 