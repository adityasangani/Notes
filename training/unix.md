# UNIX
/bin: bin directory contains the commands and utilities used by user day to day. These are executable "binary files".
/lib: this directory contains libraries that are used by programs.

## Difference between UNIX and LINUX
LINUX is a version of UNIX only. 

![image](https://github.com/user-attachments/assets/dbbfd65a-32e3-4764-8daa-5ce326b9c3e1)
1. True
2. True
3. False. Parent is at the top.
4. True

## UNIX commands
1. To write to a file
a.
```
vi demo.txt : to create a file and write in it.
```
Now to insert text/ enter data: press "i"
    Esc, 
```
    :wq //to write and come out.
```
b.   Another way of creating a file: nano demo.txt
    Ctrl + X to come out of the file. Then press Y and enter. 

c. Use cat > to create a file. Then write in it directly, and then press 'Ctrl + d' to exit.
```
  cat > demo2.txt
```

2. To create a directory: 
```
  mkdir karthik
```

To delete a file:
```
rm demo2.txt
```

4. To delete a directory
```
rmdir karthik
```

5. To come to the home directory directly:
```
cd ~
cd ../../
```

6. To find out the files that were recently created
```
ls -lt
ls -lrt //isme recent waale end mein aayenge
ls -lS //max size waale pehle aayenge
ls -lrS //max size waale last aayenge (reverse)
ls -lh //how much kb is in each file
```

7. How to read file content directly
```
cat demo.txt
```

8. To copy content from one file to another. We can also create a new file with this command. 
```
cp demo.txt demo2.txt //demo.txt's data will be copied into demo2.txt.
```

9. To compare the files.
```
  vimdiff demo.txt demo2.txt
```
To come out, esc + :wq + enter

10. To find a file
```
find ~ -type f -name 'sample.txt' //here '~' is for searching from the home directory. We can also replace that with a directory to search for files into it.
find . -type d -name 'Practices' //d here is for finding directory. f in the above command is for file. I guess here dot means from this current position find the file.
```

11. We can read multiple files simultaneously
```
cat demo.txt demo1.txt sample.txt
cat -n demo.txt demo1.txt //to find number of lines also
```

12. To list out few lines (by default 10) of a file
```
head demo.txt //first 10 lines
head 3 demo.txt //first 3 lines
tail -3 demo.txt //last 3 lines
```

13. To change the permissions of a directory and file (read, write and execute permission)
```
ls -l // to see permissions
chmod 700 demo.txt //removing all permissions
chmod go+r demo.txt //read permission given
chmod go+w,go-r demo.txt //giving write permission and removing read permission
chmod go+rwx demo.txt //giving all three, read, write and execute permissions
```

14. To search content from a particular file
```
grep <enter word> demo.txt
grep <enter word> demo.txt demo2.txt demo3.txt
```
