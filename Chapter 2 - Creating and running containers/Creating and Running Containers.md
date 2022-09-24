# Why containers?

Applications are typically comprised of a language runtime, various libraries and the source code.

Libraries may require external libraries such as libssl to run. These libraries are usually shipped with the OS that is installed on a particular machine.

If the app has a dependency on a library that is installed on the developers workstation but not on the application server you get the "but it runs on my machine" error when trying to deploy code.

A program can only only be considred reliably when it can be reliably deployed to any system and function as expected.

When doing an imperative deployment you are at an increased risk of running into these problems because you will have a workflow like below

>1. Connect to remote machine
>2. Download source code from location A
>3. Patch Components 1 -3
>4. Update Library F
>5. Install source code
>6. Run application

If anything in the above steps changes from the time the developer writes the code to when the application is installed you may have problems.

This can include updates to the libraries you are installing and just creates needless complexity in the deployment process.

