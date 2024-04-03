# Challenge: Permissions and Package managers

## Intro & Initial Set Up

Though there are similirities to how we can set up our environment between a MacOS, Linux and WSL on Windows machine, there are limitations to what we can consistently do out of the box in all three of these set ups.  This is where we can look to containerization as a means of implementing finer environmental control.  By running Linux through an Ubuntu (a distribution of Linux) image in a docker container, we can run the same exercise across various machines without having to make major changes to the machine environment itself.

If you're new to docker, we recommend starting with Docker Desktop, which you can download from [here](https://www.docker.com/products/docker-desktop/).

Once you're set up, clone this repository onto your local machine.  With Docker running in the background, you'll be able to follow this exercise's instructions in setting up a dockerized instance of Ubuntu.  

## Exercise:  Users, Groups and Permissions in an Ubuntu Environment

1. Let's start by creating our own shell script to run these docker commands.  Using the command line, create a file called 'start.sh' and add the following lines your new bash script:

2. Add the following to this script:
```sh
#!/bin/bash

docker build -t bash-container .
docker run -it --rm --name bash-container bash-container
docker rmi bash-container
```

> You should recognize these steps from the previous command, only this time, we've removed the comments detailing each line of the dockerfile.  As you type out (or paste) these commands, think through each command.  Can you articulate what is happening at each step and why certain parameters are being passed in?

3. Execute the script.

<details>
  <summary>Hint</summary>
  Before you can run this script, you may find that you'll need to modify the **permissions** on this file so that it is executable. 
</details>

<details>
  <summary>Hint</summary>
  Once you've added execute permissions, you can run `./start.sh`.  Notice the command requires us to begin with a relative path prior to calling the file by its name.  If you were to try `start.sh`, it would fail.  Why is this?
</details>

Running the script should move you into a new environment.

### Adding Users

Do you notice your username in this new environment?  This is the default user that's created when spinning up a Linux instance.

4. Recall that UNIX systems are multi-user systems.  Let's navigate to the directory where different user directories are stored.

<details>
  <summary>Hint</summary>
  This directory should be `/home`, and it should currently be empty.
</details>

5. There are a few different commands for adding users.  Let's try each so we can compare the two.
   a. Add a user using `adduser`
   b. Add a second user using `useradd`

What differences did you notice in the command line?  What other differences can you see within the home directory?

<details>
  <summary>Explanation</summary>
  While both `adduser` and `useradd` can add users, the first command will automatically generate a `/home/user` directory for anyone currently signed in as a system user.  The command `useradd` only adds home directories for normal users, and you're currently signed in as `root`.
</details>

6. Users are essentially *entities* or *entries* in the operating system.  As such, we use the `getent` command to look them up (there are actually many different ways with variations based on the distribution and version of Linux being used, but getent is the easiest to remember and use).

> getent passwd
 
This command will pull all users, which are also stored in an /etc/passwd file in Linux.

Notice how the data follows this format
> `<username>:<password>:<UID>:<GID>:<GECOS>:<home_dir>:<shell_path>`

Username and password are self explanatory.  UIDs (user) and GIDs (groups) are unique identifiers that can be used to look up the user.  GECOS refers to additional information and comments about the user.  Finally, we see the user's home directory and the login shell of the user.

We can also use the `getent` command to grab users individually.  All you need to do differently is pass in the username you're searching for: `getent passwd <username>`

### Installing NodeJS

7. You should have already installed NodeJS on your local machine.  Do you remember what node command to use to spin up a node environment?  Let's make confirm we can spin up a node environment before proceeding.

<details>
  <summary>Hint</summary>
  Yes, this was sort of a trick question.  Regardless of whether or not you recalled the correct node command, you're not currently working out of your local machine... You're signed in as the root user to an Ubuntu instance inside a docker container.  Yes, this technically exists on your local machine, but part of what containers are designed and used for is the intentional segregation of environment control.  Remember the diagram we used to explain UNIX architecture?  Think of this container as being a bubble inside the application layer. You ran shell commands to instantiate an application (Docker) that spins up a containerized virtual environment (Ubuntu).  

 Since NodeJS is not not installed in this bubble, you'll want to use apt install to install `nodejs`.

</details>

<details>
  <summary>Hint</summary>
  The command to install nodejs isn't working, is it?  Consider the error message you're receiving and why it is.  The answer isn't very complicated, it really is just because this is by design.  Apt as a package manager can only run packages it has access to, and that means it must  already exist somewhere in your Linux environment.  By default, it doesn't, because the distribution of linux we selected does not come packaged with it.  You'll have to retrieve the nodejs package, which you can do manually, or automatically via `apt update`.

 The install may prompt you about disk space usage.  Don't worry about this.  One additional benefit to working within a container is how easy it is to clean up.  We've written today's challenge in a way that once you exit the container, it will automatically disappear into the ether, leaving behind not a single trace that it had ever existed (a bit of a dramatization, but for just about right for all practical intents and purposes).
</details>

8.  Once we have NodeJS installed, we'll want to create a javascript file to call and execute.  Name the file something like 'hello.js' and open it up.

<details>
  <summary>Hint</summary>
  If you're trying to use `code`, you'll find that it doesn't work.  You'll need to use a text editor like the one we mentioned in today's lesson.  Like NodeJS, it doesn't come preinstalled in the Ubuntu image we've used to build this container.  It should have been pulled however by the update you ran earlier, so you shouldn't need to run the update again.  Install this text editor the way you installed NodeJS.
</details>

9.  Inside this file, write some simple log statements.

<details>
  <summary>Hint</summary>
  You may already be quite familiar with `console.log`, especially if you've gone through either our FullStack curriculum or Fundamentals of Programming.  But what if you try `process.stdout.write()` instead?  What's the [difference](http://nodejs.org/docs/v0.3.1/api/process.html#process.stdout) between these two commands?
</details>

10. Run your file.  If you see the output in your terminal, then you've succeeded!

### Groups

We can retrieve groups using `getent` the way we used it to retrieve users.  Try it with the 'group' keyword.  

What do you notice about the users you created earlier?  Think about how this compares to the results of the `getent passwd` command.  What does it tell you about the default behavior or users and groups when users are created?

11. Let's create a new group using either `addgroup` or `groupadd`.  Just like with `adduser` and `useradd`, there are slight differences, but these differences may not be as apparent. `addgroup` and `adduser` are actually convenience wrappers around the `groupadd` and `useradd` commands, meaning they allow for additional functionality before calling those commands. `groupadd` is generally more useful when scripting (a typical use case being bulk adding a batch of users) whereas `addgroup` is more user friendly and interactive.

12. Let's now add one of the users to your new group using `adduser`:

> `adduser` `<username> <groupname>`

Run your view commans from earlier to see what's changed.

### Permissions

So, you now know how to create users and groups in multiple ways.  But why does this ultimately matter?  When in an administrative role that requires the management of users and groups, you're likely also in charge of managing permissions for these users and groups.

13. Let's take the `hello.js` file you created earlier and use `chown <user> <file>` to assign ownership of the file to one of the users you created.
14. Now use `su <username>` to elevate into a new shell level as that user.
15. Run `chmod 700 <filename>` to restrict file access to your current user.
16. Run the file to confirm it works.
17. Exit out using keyboard shortcut `ctrl+d` or by typing `exit`.  Be extremely careful!  Only do this once so you can return to being root.  Remember, we wrote this container to self-erase upon exiting.  If you exit twice in a row, you'll lose all your progress.
18. Now sign in as the second user you've created, and try running the same file.

If you triggered an error, then you've successfully restricted the second user from accessing this file.

## Stretch / Look-ahead Assignment

[Read through this MIT "The Missing Semester of your CS Education" tutorial on shell scripting](https://missing.csail.mit.edu/2020/shell-tools/).

**Be sure to write out and run the code examples in the tutorial.**

Do the following exercises from it:

1. Write bash functions `marco` and `polo` that do the following. Whenever you execute marco the current working directory should be saved in some manner, then when you execute `polo`, no matter what directory you are in, `polo` should cd you back to the directory where you executed `marco`. For ease of debugging you can write the code in a file `marco.sh` and (re)load the definitions to your shell by executing source `marco.sh`.

2. Say you have a command that fails rarely. In order to debug it you need to capture its output but it can be time consuming to get a failure run. Write a bash script that runs the following script until it fails and captures its standard output and error streams to files and prints everything at the end. Bonus points if you can also report how many runs it took for the script to fail.

```bash
 #!/usr/bin/env bash

 n=$(( RANDOM % 100 ))

 if [[ n -eq 42 ]]; then
    echo "Something went wrong"
    >&2 echo "The error was using magic numbers"
    exit 1
 fi

 echo "Everything went according to plan"
 ```