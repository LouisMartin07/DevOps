
# CLI Basics & Files & Folders

We're going to simulate various actions while giving you a chance to practice using various CLI commands.

Use terminal commands for the following. That may mean using a terminal based text editor such as VIM as opposed to using the VS Code editor, even if you're running a terminal inside of VS Code.  

### Writing a shell script

1. Create a shell script in this project directory called 'start.sh'
2. Add the following to this script:

```
#!/bin/bash

# Build Docker image using dockerfile in current directory
docker build -t bash-container .

# Interactively Run the Docker container, and remove upon exit
docker run -it --rm --name bash-container bash-container

# Delete the Docker image upon exit
docker rmi bash-container
```

3. Execute this script.  Don't be surprised if you're unable to right off the bat.

<details>
  <summary>Hint</summary>
  Before you can run this script, you may find that you'll need to modify the **permissions** on this file so that it is executable. 
</details>

<details>
  <summary>Hint 2</summary>
  Once you've added execute permissions, you can run `./start.sh`.  Notice the command requires us to begin with a relative path prior to calling the file by its name.  If you were to try `start.sh`, it would fail.  Why is this?
</details>

### Investigating Errors

For this part of the exercise, pretend you're investing an error in response to a request ticket.  You'll want to take some notes to share with the requestor reporting what you find.  The issue the user is reporting is that they are unable to sign into the application.

4. You should find yourself in a new environment now.  You're going to want to take some notes on your local machine.  Open up a split terminal so you can do that without closing out this new environment.
5. In your local terminal, create a new markdown file.  Add a header and a brief description of what you see.
6. Start looking around for evidence as to why the user is experiencing issues.  You'll have to navigate the directories using CD and what you know about absolute and relative paths.

<details>
  <summary>Hint 1</summary>
  That evidence will likely be a log file and whatever the error in the log file is pointing to, like a configuration file perhaps? 
</details>

<details>
  <summary>Hint 2</summary>
  Remember, files and folders can be hidden.
</details>

7. Once you find some evidence, you'll want to copy that evidence back to your local machine.  

> docker cp <containerId>:/file/path/within/container /destination/file/path

For anyone that does not yet have a lot of experience with docker, you'll want to run 'docker ps' from your local terminal (not from inside the docker container) to grab the container ID.

### Can't Run Docker?

If you aren't able to get Docker to run, you can work through the original version of this challenge:

## CLI Basics & Files & Folders

Complete the following tasks, using your shell as much as possible to do the work. Refer back to examples in the lesson.

**If you get stuck - use the ["4 Before Me" Debugging Checklist!](https://docs.google.com/document/d/1nseOs5oabYBKNHfwJZNAR7GlU0zkZxNagsw63AD7XV0/edit)**

- Create a text file in your home directory named 'shakespeare.txt' containing the following text:
  "Shall I compare thee to a summer’s day?
  Thou art more lovely and more temperate."

- Rename it to 'sonnet-18.txt'

- Make a new directory in your home directory, called 'poetry'
- Move the poem file contents into the new directory
- Use the echo command to print the result

- In the poetry directory create another file, 'sonnet-29.txt' with the contents:
  "When in disgrace with Fortune and men’s eyes,
  I all alone beweep my outcast state"

- Use the cat command to display both of the poems, using a wildcard

- Use the grep command to display only those lines with 'all' in them
- Copy the sonnet-29.txt to backupPoem.txt

- Use vim to open sonnet-29.txt, change the word 'men' to 'people'
  ed sonnet-29.txt,then save and quit

- Use the `cmp` command and the `diff` command to compare sonnet-29.txt and backupPoem.txt

- Create a directory 'tmp'
- Copy all the files in the poetry directory into the tmp one

- Return to your home directory

- list all the contents of all your directories

- From your home directory, create a new file named poem3.txt inside tmp

- change to your tmp directory and rename one poem
- copy the renamed poem back to your poetry directory

- Finally, use the rm command to delete the tmp directory and all of the poems in it