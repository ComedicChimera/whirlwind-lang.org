# Hello World

## Installation

If you haven't already installed Chai, visit our [installation page](/install).
This will walk you through the short installation process and make sure you are
all set up and ready to go for this tutorial.

## Setting Up a Workspace

In order to get started programming in Chai, you will need a workspace to run
code.  

Assuming you already have the language installed, go to your development
directory of choice (wherever you want to store your Chai projects).  Then,
create and open a new directory named whatever you want your first project name
to be. Then, open your terminal and type the following command `chai mod init
<name>` and replace `<name>` with whatever you want your starting project to be
called (ideally, it should match your chosen directory name).  Note that the
name must contain only letters, numbers, and underscores and start with either a
letter or an underscore.

> This command creates a new module which is essential to building a Chai
> project; we will learn about modules much later.

After you run this command, you should see a new file called `chai-mod.toml`.
This is the configuration file for your module.  For now, you don't need to
worry about what's inside it -- the `chai` tool has already selected some
sensible default configurations for you.

In your directory, go ahead and create a new file called `main.chai`.  Once you
have put some code into the file, you can use the command, `chai run .` to build
and run your code (current working module).

If you want to simply produce an executable, you can use the command `chai build
.` to build your current module.  The executable will be created in a directory
called `bin` by default.

## Hello Chai

Now, we are going to walk you through writing your first Chai program.  This
program is going to print out `Hello, world!` to the console and is often the
first program written when learning a new language.  

To begin, open the file you created in the first section of this chapter.  You
will put all the code we write in this section in this file.  

Firstly, we need to import the function `println` that we are going to use to
print to the console.  To do this, add the following line to the top of your
file:

    import println from io.std

`io.std` is the name of the package that contains `println`.  We will talk more
about packages and importing in a later chapter -- you won't need more than this
for a while. 

Now, we need to create your main function.  This is the function that is called
at the start of your program.  This function is simply called `main` and should
be defined as follows:

    def main() =
        ...

The `=` at the end of the function denotes the beginning of the function's body
which is the code that is run when the function is called.  You'll also notice
that we begin a new indentation level when we write the body of our function.
Chai uses indentation to determine where a block begins and ends, and
indentation level to determine where statements fit into the control flow of
your program.  

Finally, we need to call the print function.  We are going to pass in the string
`"Hello, world!"`. A string is simply a piece of text -- we will talk more about
them in the next chapter.

    def main() =
        println("Hello, world!")

That's it.  We place the arguments to our function inside parentheses.  We do
not need to use any form of punctuation to end a line in Chai.  The linebreak at
the end denotes that the line is over.  Chai is generally whitespace sensitive
and uses whitespace to discern the structure of your program -- however, it is
still fairly flexible as you will see in later chapters.

If you run the above code, you will see the text `Hello, world!` printed out in
your terminal. Congratulations, you just wrote your first Chai program and took
your first step into learning a new programming language!
