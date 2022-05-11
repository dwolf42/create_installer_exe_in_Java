# Creating an EXE file for Windows in 4 easy steps with jpackage

As being a beginner in Java myself, I always wanted to have my console program to be standalone run-able on my and the Windows computers of my friends by a single double click. There are a lot of tools and even more, sometimes good, sometimes bad tutorials on how to use these tools. Some developers even charge theirs software, which is totally fine, but they had apparently not even thought about writing a guide about how to actually use this software.

Which one I exactly used for my last project TicTacToeSuper I can’t clearly remember, but I think it was either WinRun4J or JSmooth. They did what they should and created an `EXE` file for my game, which I then could share with friends and people out there on the internet.  
  

But it also had a major downside.  
The user was required to install Java - and in some cases friends told me that they also had to install JDK.  
  
It isn’t quite comfortable if someone just wants to play my game.  
  
For my game [TicTacToeSuperGT](https://github.com/Kabraxis/TicTacToeSuperGT) I wanted to ship a game that was just running, so everyone, no matter if they have a good understanding about computers or not, will be able to play it. Because of this, I was looking for something to convert my game to an executable and also comes along with every aspect of Java that is required, but doesn’t require Java to be installed.

I found most of the tutorials about how to make an `EXE` file of my Java application, as well as the tutorials for the regarding tools, have been a nightmare for me.  
So I thought I would just write a tutorial myself. One that is understandable and that everybody can follow easily to have their program as an `EXE` file at the end.  
  
Through the tip of a Reddit user I became aware of `jpackage`, that comes with JDK and is capable of making an `EXE`, and brings all the necessary files, so the program will run without having Java necessarily installed.  
Two other pretty cool features are the options for the user to decide in which directory my program should be installed, and running the installer after the program was installed will offer the options to either repair or even remove the application.

Oh, and it's also relatively easy to use once you've seen how it works.

Now without further ado, let’s jump right into the tutorial!  
  
<br/>


## **Prerequisites:**

**Create:**
A new empty folder on your desktop, you can name it as your software. For the sake of simplicity, I use `Main` as name for my project, and I will write most commands to work with Main as project and folder names.  
If you want to have another name, please change Main into whatever your project’s name is.
    
What to place in that folder?
 
     
- [ ] The `JAVA` file of your project (`Main.java`).  
- [ ] An `ICO` file, which you want to have as icon for your application.  
- [ ] *Note:* `ICO` max file size is 150 x 150 pixels.  
- [ ] Convert your image to `ICO` on [https://convertio.co/](https://convertio.co/)
- [ ] Another `ICO` file, which you want to have as icon for the installer of your program.

    
**Install:** 
- [ ] Wix Toolset [https://wixtoolset.org/releases/](https://wixtoolset.org/releases/) - jpackage needs it to be able to create your Windows Installer.

- [ ] Microsoft .NET Framework 3.5 Service Pack 1 https://www.microsoft.com/en-US/download/details.aspx?id=22 - which Wix requires to be installed. 
    
- [ ] 7-Zip [https://7-zip.org/](https://7-zip.org/) - we need this to specify the main class inside the JAR file we are going to create.
    
- [ ] Resource Hacker [http://www.angusj.com/resourcehacker/](http://www.angusj.com/resourcehacker/) - used to change the icon of the installer.
    

At this point, I assume you already have installed the JDK (Java Development Kit), as well as the JRE (Java Runtime Environment).  
In case there is one of them missing, you can get both here:  

- [ ] JDK: [https://www.oracle.com/java/technologies/downloads/](https://www.oracle.com/java/technologies/downloads/)
- [ ] JRE: [https://java.com/en/download/](https://java.com/en/download/)  
         
      
<br/>

## 1# Compiling the CLASS file:
   
-   Open Windows start menu, type CMD and run it.
    
-   The standard directory you are currently in is usually `C:\Users\YOUR_USERNAME` (on Win10 at least).
    
-   Navigate to your project’s folder, type `cd Desktop\Main` into the CMD and hit `enter`.
    
-   In order to compile your project to bytecode, type `javac Main.java` and, again, hit `enter`.
    
-   Now we have the `Main.class` in our folder at the desktop. You can check this by typing `dir` into CMD, followed by enter. This lists all files inside that folder.  
      
    
    <br/>

## 2# Creating a JAR file, which already contains the main class
      
    

-   To create the `JAR` file that contains a manifest file, type `jar cfe Main.jar Main.class` into CMD, and push the `enter` key.  
      
    >**What this command does:**  
    *`jar` tells CMD to use the jar program coming with JDK.  
    `c` option indicates that you want to create a `JAR` file.  
    `e` sets an entrypoint as the application entry point for stand-alone applications bundled into executable jar file, without editing or creating the manifest file.<br/>
    `f` option indicates that you want the output to be a file.  
    `Main.jar` is the name that your `JAR` will have after creation. You can name it anything, as long as it ends with `.jar` (basically you could even use another ending than `jar`, but that’s no good practice).  
    `Main.class` specifies the file you want to put into your JAR.  
    For more info about these and other commands, have a look at:* [https://docs.oracle.com/javase/tutorial/deployment/jar/build.html](https://docs.oracle.com/javase/tutorial/deployment/jar/build.html)  
      
    
<br/>


## 3# Let’s create the installer
    

Great job so far! You have mastered the hardest part, and we are close to have our installer ready.

-   Making an installer using `jpackage` will cause `jpackage` to put *every file* that is inside the same folder as the `JAR` file into the installer. This unnecessarily bloats the installer, resulting in a much bigger file size as actually needed. So please `open` your project folder on your desktop, create a `new folder`, name it `output`, and put your `JAR` inside.
    
-   Now we are ready to create a Windows Installer, which installs our program as an `EXE` on a computer. As I said, it will let the user decide where they want to install it, and also ships everything needed for our Java program to run.  
Which means, the users doesn’t have Java to be installed, which is pretty cool, if you ask me.
    
-   The commands we have to enter into CMD for jpackage to create an installer is pretty long, and you will have to `edit` it, so it fits your path/name. Because of this, I recommend you to `copy` it to a text file. This way you can also save it somewhere on your computer, and use it as needed.  
      
    
-   Your CMD is still in the directory of your project, so let’s navigate to the output folder we just created, by typing `cd output` into the console, and hit `enter`.  
The is how it should be displayed in your CMD `C:\Users\YOUR_USERNAME\Desktop\Main\output`
      
    
-   `Paste` the following command to your text file and `change` it to your Windows username.  

> **In case your project is not located at your desktop, has a different
> name, or you just want to have your program a different name than
> Main: I will explain every part of the command below it, so you can
> edit it as needed.**

      
-   If you have `edited` everything, `paste` your command into CMD by `CTRL + V` or `CTRL + INS` for jpackage to start doing its job:  
      
`jpackage -t exe --name Main --description "This is my amazing Java program” --app-version 1.0 --input C:\Users\YOUR_USERNAME\Desktop\Main --dest C:\Users\YOUR_USERNAME\Desktop\Main\output --icon C:\Users\YOUR_USERNAME\Desktop\Main\NameOfYourProgramsIcon.ico --main-jar Main.jar --win-console --win-dir-chooser --win-shortcut`
    

**Please note:** *jpackage needs a couple of moments (depending on how fast your computer is) to create the installer.*

<br/>

**What are all these commands good for?**  

`jpackage`: Runs jpackage (obviously).  
`-t exe`: Tells `jpackage` what type of package to create, in our case an `EXE` file.  
`--name`: Specifies what `name` the application and installer should have.  
`--description`: Describe what your program is, take care about putting it into `“ “` - like a String variable.  
`--app-version`: The version of your app.  
`--input`: Path to the files to put into the installer, laying in your project’s folder. This means, every file that’s in this folder will be packed into the `EXE`.  
`--dest`: Path where the output installer should be saved.  
`--icon`: Path of your icon that you want the program (not the installer!) to have.  
*Note: ICO max file size is 150 x 150 pixe*ls.  
`--main-jar:` Name of your.  
`--win-console`: It specifies to create a console launcher for the program, in case it is a program that runs in the console.  
`--win-dir-chooser`: Allows the user to choose where to install the application.  
`--win-shortcut`: Puts a shortcut of your program to the user’s desktop.

There are also options for JavaFX applications, just replace -`-win-console`with `--module-path`, as well adding `--add-modules`.  
For further detail, please visit: [https://docs.oracle.com/en/java/javase/14/docs/specs/man/jpackage.html  ](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jpackage.html)

<br/>

## 4# New icon for the installer  
      
   
-   So, there is your `Main-1.0.exe` file inside your project’s `folder`. It usually has the same icon as your actual program. I personally don’t like it when the installer and the program share the same icon, as they can easier mistake with each otter **🦦**.  
And right here comes Resource Hack into play.
    
-   `Run` Resource Hack from its desktop or start menu shortcut.
    
-   Click `File` - `Open` … - navigate to your project’s folder and select your `Main-1.0.exe` - click `open`.
    
-   Click `Action` - `Add an Image or Other Binary Resource…`, in the upcoming window click Select `File…`, select the `ICO` file for your installer and click on `Add Resource`.
    
-   In the `main window` of Resource Hack, you see `two green floppy discs` (for the younger people aka. `save icons`), `click` the left one in order to `save` everything. Now you can `close` Resource Hack.
    
<br/>

And there it is, your installer is ready and coming with an individual icon - congratulations! 🎉
<br/>

<br/>


# TROUBLE SHOOTING:

**`JAR` command not recognized in CMD:**  
  
1. This can be related to a missing installation of the `JDK` (Java Development Kit), please see *prerequisites* for the link. I highly recommend you to install it in the default directory (as the installer suggests), that way you make things easier in setting the Environment Variable aka. include `JDK` into the `PATH`.

2. `PATH` entry / `Environment Variable` is missing can also cause CMD not knowing the `JAR` command. To change them, `open` Windows start menu and type `environ` this should be enough to bring up `Edit the system environment variables` - run it.

-   Go to the `Advanced` tab (top side) and click `Environment Variables…`
    
-   In the lower part of the upcoming window, you find the `System variables`, look for the entry named `Path` (*take care, there is another entry with a similar name*), `highlight / single click` it and click `Edit…`
    
-   There should be a variable called `C:\Program Files\Java` or similar. Mine contained something with *Oracle* in it. `Select it` and click `Browse…`
    
-   Now, navigate to `C:\Program Files\Java\` - select the `Bin` folder - hit 4 times `OK`, once  in each window.
    
-   In case your CMD is open, `close` and `re-open` it. You now can type `jar` into CMD, and when it displays something like `Usage: jar [OPTION…]` etc. you’re fine now.
    
<br/>

**Antivirus suspects the installer / program as malware:**  
  
This is totally normal, as your program is self-written, so no antivirus program knows about it. Just let the AV scan the program, and it should be fine to run.  Sometimes white-listing the installer or program can also help to run it.
  
While in theory you could send your program to some of the most common AV companies to verify if that it is safe, this would only apply for this one version. The next time you update your application to a newer version, you will have to re-do it again.

As far as I know, the most elementary way to avoid AVs from going mad about your program is to get a real certificate for Windows for it, but this will cost money.

A good workaround is to put your installer into a folder and make a ZIP or RAR file out of it. This way you can place a `readmefirst.txt` which the user - hopefully - opens and reads before running the program.  
But we all know, no one reads these files ;)  
  
The best practice is to tell people about the AV will notice the installer, and after it scanned it, everything will be fine.
