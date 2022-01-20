# create_installer_exe_in_Java
A tutorial about how to create a Windows installer for your Java console program (infos for JavaFX)
<!-- Output copied to clipboard! -->

<!-----



Conversion notes:

* Docs to Markdown version 1.0β33
* Thu Jan 20 2022 13:49:10 GMT-0800 (PST)
* Source doc: Kopie von Java Sheet
* This is a partial selection. Check to make sure intra-doc links work.
----->


**<span style="text-decoration:underline;">Creating an EXE file for Windows with jpackage</span>**

As being a beginner in Java myself, I always wanted to have my console program to be standalone run-able on my and the Windows computers of my friends by a single double click. There are a lot of tools and even more, sometimes good, sometimes bad tutorials on how to use these tools. Some developers even charge theirs software, which is totally fine, but they had apparently not even thought about writing a guide about how to actually use this software.

Which one I exactly used for my last project TicTacToeSuper I can’t clearly remember, but I think it was either WinRun4J or JSmooth. They did what they should and created an EXE file for my game, which I then could share with friends and people out there on the internet. \


But it also had a major downside. \
The user was required to install Java - and in some cases friends told me that they also had to install JDK. \
 \
It isn’t quite comfortable if someone just wants  to play my game. \
 \
For TicTacToeSuperGT I wanted to ship a game that was just running, so everyone, no matter if they have a good understanding about computers or not, will be able to play it. Because of this, I was looking for something to convert my game to an executable and also comes along with every aspect of Java that is required, but doesn’t require Java to be installed.

I found most of the tutorials about how to make an EXE file of my Java application, as well as the tutorials for the regarding tools, have been a nightmare for me. \
So I thought I would just write a tutorial myself. One that is understandable and that everybody can follow easily to have their program as an EXE file at the end. \
 \
Through the tip of a Reddit user I became aware of jpackage, that comes with JDK and is capable of making an EXE, and brings all the necessary files, so the program will run without having Java necessarily installed. \
Two other pretty cool features are the options for the user to decide in which directory my program should be installed, and running the installer after the program was installed will offer the options to either repair or even remove the application.

Oh, and it's also relatively easy to use once you've seen how it works.

Now without further ado, let’s jump right into the tutorial! 

 
Prerequisites: 



* Create: A new empty folder on your desktop, you can name it as your software. For the sake of simplicity, I use the Main as name for my project, and I will write most commands to work with Main as project and folder names. \
If you want to have another name, please change Main into whatever your project’s name is.
* Things to put inside that folder:  
- [ ] The JAVA file of your project (Main.java). 
- [ ] An ICO file, which you want to have as icon for your application. \
- Note: ICO max file size is 150 x 150 pixels. \
Convert your image to ICO on [https://convertio.co/](https://convertio.co/) 
* >Another ICO file, which you want to have as icon for the installer of your program.
* Install: Wix Toolset [https://wixtoolset.org/releases/](https://wixtoolset.org/releases/) - jpackage needs it to be able to create your Windows Installer.
* Install: 7-Zip [https://7-zip.org/](https://7-zip.org/) - we need this to specify the main class inside the JAR file we are going to create.
* Install: Resource Hacker [http://www.angusj.com/resourcehacker/](http://www.angusj.com/resourcehacker/) - used to change the icon of the installer.
* At this point, I assume you already have installed the JDK (Java Development Kit), as well as the JRE (Java Runtime Environment). \
In case there is one of them missing, you can get both here: \
JDK: [https://www.oracle.com/java/technologies/downloads/](https://www.oracle.com/java/technologies/downloads/) \
JRE: [https://java.com/en/download/](https://java.com/en/download/)  \

1. <span style="text-decoration:underline;">Compiling the CLASS file \
</span>
* Open Windows start menu, type _CMD_ and run it.
* The standard directory you are currently in is usually _C:\Users\YOUR_USERNAME_ (on Win10 at least). 
* Navigate to your project’s folder, type _cd \Desktop\Main_ into the CMD and hit enter.
* In order to compile your project to bytecode, type _javac Main.java_ and, again, hit enter.
* Now we have the Main.class in our folder at the desktop. You can check this by typing _dir_ into CMD, followed by enter. This lists all files inside that folder. \

2. <span style="text-decoration:underline;">Creating a JAR file \
</span>
* To create the JAR file that contains a manifest file, type _jar cf Main.jar Main.class_ into CMD, and push the enter key. \
 \
<span style="text-decoration:underline;">>What this command does:</span> \
_jar _tells CMD to use the jar program coming with JDK. \
_c_ option indicates that you want to create a JAR file. \
_f_ option indicates that you want the output to be a file. \
_Main.jar_ is the name that your JAR will have after creation. You can name it anything, as long as it ends with .jar (basically you could even use another ending than jar, but that’s no good practice). \
_Main.class_ specifies the file you want to put into your JAR.  \
For more info about these and other commands, have a look at: [https://docs.oracle.com/javase/tutorial/deployment/jar/build.html](https://docs.oracle.com/javase/tutorial/deployment/jar/build.html)  \

3. <span style="text-decoration:underline;"> Specify the main class</span>

Ok, this part is a little tricky, so make sure you read it carefully, <span style="text-decoration:underline;">before </span>following the steps. 



* Go to your project’s folder, right-click the _Main.jar_ - click _7-Zip _-_ open_
* Open the _META–INF_ folder_ _- open _MANIFEST.MF \
 \
_>The file contains text, similar to this: \
_Manifest-Version: 1.0 \
Created-By: 17.0.1 (Oracle Corporation)_

    **Warning: The manifest must end with a new line or carriage return after the text. The last line will not be parsed properly if it does not end with a new line or carriage return.**


Add the last line Main-Class: Main \
	After this line there has to be a new line or carriage return! \
 \
	_Manifest-Version: 1.0 \
	Created-By: 17.0.1 (Oracle Corporation) \
	_Main-Class: Main


    **Warning: The manifest must end with a new line or carriage return after the text. The last line will not be parsed properly if it does not end with a new line or carriage \
	return. \
**	



* OK then. Now, let’s hit _File_ - _Save_ and close the document. 7-Zip will ask you to update the archive, hit _OK_ once again and close the 7-Zip window. \

4. <span style="text-decoration:underline;"> Let’s create the installer</span>

Great job so far! You have mastered the hardest part, and we are close to have our installer ready.



* Making an installer using jpackage will cause jpackage to put every file that is inside the same folder as the JAR file into the installer. This unnecessarily bloats the installer, resulting in a much bigger file size as is actually needed. So please open your project folder on your desktop, create a new folder inside called _output_, and place your JAR in it.
* Now we are ready to create a Windows Installer, which installs our program as an EXE on a computer. As I said, it will let the user decide where they want to install it, and also ships everything needed for our Java program to run.  \
Which means, the users doesn’t have Java to be installed, which is pretty cool, if you ask me.
* The commands we have to enter into CMD for jpackage to create an installer is pretty long, and you will have to edit it, so it fits your path/name. Because of this, I recommend you to copy it to a text file. This way you can also save it somewhere on your computer, and use it as needed. \

* Your CMD is still in the directory of your project, so let’s navigate to the output folder we just created, by typing _cd output_ into the console, and hit enter. \
The displayed in CMD should look like _C:\Users\YOUR_USERNAME\Desktop\Main\output_._ \
_
* Paste the following command to your text file and put in your Windows username. \
<span style="text-decoration:underline;">In case your project is not located at your desktop, has a different name, or you just want to have your program a different name than Main: I will explain every part of the command below it, so you can edit it as needed.</span>   \

* If you have edited everything, paste your command into CMD by CTRL + V or CTRL + INS for jpackage to start doing its job: \
 \
_jpackage -t exe --name Main --description "This is my amazing Java program” --app-version 1.0 --input C:\Users\YOUR_USERNAME\Desktop\Main --dest C:\Users\YOUR_USERNAME\Desktop\Main\output --icon C:\Users\YOUR_USERNAME\Desktop\Main\NameOfYourProgramsIcon.ico --main-jar Main.jar --win-console --win-dir-chooser --win-shortcut_ 

    Please note: The jpackage needs a couple of moments (depending on how fast your computer is) to create the installer.


    What do all those commands? \
> jpackage: Runs jpackage (obviously). \
> -t exe: Tells jpackage what type of package to create, in our case an EXE file. \
> --name: Specifies what name the application and installer should have. \
> --description: Describe what your program is, take care to put it into “ “ - like a String variable. \
> --app-version: The version of your app. \
> --input: Path to the files to put into the installer, laying in your project’s folder. This means, every file that’s in this folder will be put into the EXE. \
> --dest: Path where the output installer should be saved. \
> --icon: Path of your icon that you want the program (not the installer!) to have. \
Note: ICO max file size is 150 x 150 pixels. \
> --main-jar: Name of your. \
> --win-console: It specifies to create a console launcher for the program, in case it is a program that runs in the console. \
> --win-dir-chooser: Allows the user to choose where to install the application. \
> --win-shortcut: Puts a shortcut of your program to the user’s desktop.


    There are also options for JavaFX applications, just delete --win-console and put in --module-path, as well as --add-modules. \
For further detail, please visit: [https://docs.oracle.com/en/java/javase/14/docs/specs/man/jpackage.html](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jpackage.html) \


5. <span style="text-decoration:underline;">New icon for the installer \
</span>
* So, there is your Main-1.0.exe file inside your project’s folder. It usually has the same icon as your actual program. I personally don’t like it when the installer and the program have the same icon, as it is easier to mistake one and the other. \
Here comes Resource Hack into play.
* Run Resource Hack from its desktop or start menu shortcut.
* Click _File_ - _Open …_ - navigate to your project’s folder and select your Main-1.0.exe - click _open_.
* Click _Action_ - _Add an Image or Other Binary Resource …_, in the upcoming window click _Select File …_, select the ICO file for your installer and click on _Add Resource_.
* In the main window of Resource Hack, you see two green floppy discs (aka. save icons), click the left one in order to save everything. Now you can close Resource Hack.

And there it is, your installer is ready and coming with an individual icon.

TROUBLE SHOOTING:

JAR command not recognized in CMD: \
 \
1. This can be related to a missing installation of the JDK (Java Development Kit), please see prerequisites for the link. I highly recommend you to install it in the default directory (as the installer suggests), that way you make things easier in setting the Environment Variable aka. include JDK in the PATH.

2. PATH entry / Environment Variable is missing can also cause CMD not knowing the JAR command. To change them, open Windows start menu and type _environ_ this should be enough to bring up _Edit the system environment variables_ - run it.



* Go to the _Advanced _tab (top side) and click _Environment Variables…_
* In the lower part of the upcoming window, you find the _System variables_, look for the entry named _Path _(take care, there is another entry with a similar name), _single click_ it and click _Edit…_
* There should be a variable called _C:\Program Files\Java_ or similar. Mine contained something with _Oracle _in it. Select it and click _Browse…_
* Now, navigate to _C:\Program Files\Java\_ - select the _Bin_ folder - hit 4 times _OK_, once_ _in each window.
* In case your CMD is open, close and re-open it. You now can type _jar _into CMD, and if it displays something like _Usage: jar [OPTION…]_ etc. you’re fine now.

Antivirus suspects the installer as malware: \
 \
Your program is self-written, so no antivirus program knows about it. Just let the AV scan the program, and it should be fine to run. \
 \
While in theory you could send your program to some of the most common AV companies to verify it is safe, this would only apply for this one version. If you update your application to a newer version, it has to been done again.

As far as I know, the best way to avoid AVs from going mad is to get a real certificate for Windows for it, but this will cost money.

A good workaround is to put your installer into a folder and make a ZIP or RAR archive out of it. This way you can place a readmefirst.txt which the user - hopefully - opens and reads before running the program. \
But we all know, no one reads these files ;) \
 \
The best practice is to tell people about the AV will notice the installer, and after it scanned it, everything will be fine.
