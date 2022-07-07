Exercise: Create A Basic Makefile
Makefiles can be incredibly useful ways to build up a chain of commands that build, test and deploy software.

Now that you've seen an example of a Makefile, you are ready to create one of your own. Makefiles have plenty of different commands and variables that can be used - check out this page for a long list of them.

https://makefiletutorial.com/


While you will start from scratch in this exercise, if you need to look at another example of a Makefile, you can check out this Makefile in the course repository. This additional example Makefile will be used in the next lesson on containerization.

https://github.com/udacity/DevOps_Microservices/blob/master/Lesson-3-Containerization/Makefile


Instructions
Create an empty Makefile using the touch command

Create two commands: cmd1 and cmd2 and make this echo the name of the command.

Note that Makefiles will normally print out each line as it goes, but here we want it to just explicitly echo the command names. Check out the link above to figure out how you can stop a line from printing.

Makefiles have an automatic variable that contains the target name for the commands you create. You can also find this in the link above to add to your Makefile.

Create an all command that runs both cmd1 and cmd2
Run make all to see the output

One Potential Solution
Here is an example of a Makefile you could create for this exercise:
cmd1:
        @echo "$@"

cmd2:
        @echo "$@"

all: cmd1 cmd2

Note that after cmd1 and cmd2, and before @echo, should be a tab. The @ at the start of these lines prevents make from automatically printing the lines, while "$@" is the variable for a string containing the target name, in this case either cmd1 or cmd2. To double-check that make is actually showing the command name from within the command itself, try to echo something else from within one of them, such as Hello World!, and check the results.

https://github.com/udacity/DevOps_Microservices/blob/master/Lesson-3-Containerization/Makefile