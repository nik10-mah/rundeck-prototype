# Scope of Project
Evaluate Rundeck as an effective user interface on top of existing Java command line / server programs

Rundeck - https://github.com/rundeck/rundeck describes itself as

Rundeck is an open source automation service with a web console, command line tools and a WebAPI. It lets you easily run automation tasks across a set of nodes.

The scope of this project is to prototype Rundeck as being effectively used as a user interface / API interface on top of an existing Java commandline and server program. The existing Java program is https://github.com/distributev/mailmerger

# Project Deliverables

1. Required project source code pushed to this github repository
2. https://github.com/distributev/rundeck-prototype/blob/master/mailmerger-assembly/build.gradle script which will generate <a href="#mailmergerzip">mailmerger.zip</a> file described below
3. Document with detailed and clear steps how to achieve everything described in the below section called <a href="#testing">How Testing Will Be Done</a>
4. [rundeck-customizations-guide.md](rundeck-customizations-guide.md) document with separate paragraphs which explain the steps required (with screenshots)
 * How to change the logo in rundeck user interface?
 * How to customize existing rundeck jobs-list and job-details screens? (add / hide job fields in the UI) 
 * How to customize the rundeck color scheme?
 * How to add new menu items in the rundeck menu bar? How to hide / remove menu items from the rundeck menu bar?
 * How to add new UI components in the rundeck user interface? How to hide / remove UI components from the rundeck user interface?

<a name="mailmergerzip"><h2>mailmerger.zip</h2></a>

![image](https://user-images.githubusercontent.com/19224635/40276491-d26562f0-5c0b-11e8-8dfd-365289e72aba.png)

# What already exists 

The existing https://github.com/distributev/mailmerger Java program is composed of 2 parts

* a command line which takes some input parameters and does some normal processing and also generates info.log and errors.log outputs
* a server program which "wraps" the same processing as the command line but in an "automated" polling (server) way of execution - the same info.log and errors.log are generated by the server also

Both mailmerger command line and mailmerger server take as input CSV files and generate PDF output files. mailmerger server once started is continuously polling a "poll" folder for new CSV files to be processed - once new CSV files are dropped into the "poll" folder (configurable) the CSV files are automatically picked and processed to generate the output PDF files.

# What needs to be done

Using rundeck build a simple https://github.com/distributev/mailmerger user interface which will allow users:

1. to browse and select local CSV files to be uploaded to the "poll" folder which is watched by the existing mailmerger-server
2. once new CSV files are uploaded automatically pick and "wrap" the existing processing using the http://rundeck.org/docs/api/ ==> the existing mailmerger processing will be "wrapped" as rundeck jobs
3. once the jobs are processed using rundeck apis allows users to see the output of the jobs inside the user interface. For instance the users should be able to see a simple list with the CSV jobs they triggered along with the status which could be "green success" or "red errors"
4. Once clicking on each individual job in the list of previously executed jobs ==> a job details page should open which will display, in the case of successful job, a list with the individual PDFs generated (along with a link to view / download each PDF) and in the case of error the job details should display the errors.log specific to the current job execution (users should be able to see for what reason the specific job failed).

The processing along with the logging (errors / info) are already handled by the existing mailmerger cmdline / server https://github.com/distributev/mailmerger the current prototype should only put a "rundeck powered" user inteface on top of existing processing.

<a name="userinterface"><h2>User Interface Guidelines</h2></a>

The prototyped user interface / screens should contain the following capabilities / information while the actual look and feel should come from rundeck (use rundeck to provide capabilities displayed here)

![image](https://user-images.githubusercontent.com/19224635/40109868-bdd5e3ec-58fe-11e8-9989-44bbfad2d7c3.png)

<a name="testing"><h2>How Testing Will Be Done</h2></a>

<strong>Step 1</strong> - Build mailmerger.zip using the script provided in mailmerger-assembly/build.gradle file

<strong>Step 2</strong> - Unzip mailmerger.zip and start the mailmerger jobs console server using the script startMailMerger.bat - the jobs console should become accessible through a web browser

<strong>Step 3</strong> - Unzip mailmerger.zip and start the mailmerger web console server using the script startMailMerger.bat - the web console should become accessible through a web browser

<strong>Step 4</strong> - Using the web interface upload a CSV file which should be picked and processed without errors. The new job should become available in the job list and the status should become COMPLETET after few seconds. Clicking the job details should open a new screen with all the details of the job and it should be possible to view and download each of the ouput PDF files.


## Steps to Implement the Above Requirements

<strong>Step 1</strong> - Fork existing https://github.com/distributev/mailmerger project

<strong>Step 2</strong> - Create the new mailmerger-jobs-webconsole and mailmerger-assembly folders;  mailmerger-jobs-webconsole will contain rundeck customizations specific to this project; mailmerger-assembly build.gradle script will take as input mailmerger + rundeck core + mailmerger-jobs-webconsole rundeck customizations and will generate the mailmerger.zip described above; mailmerger-assembly build.gradle will have defined a constant with the link to download rundeck (i.e.http://dl.bintray.com/rundeck/rundeck-maven/rundeck-launcher-2.11.3.jar) 
* if the specified rundeck download is already available locally then the local version will be used to generate mailmerger.zip
* otherwise rundeck will be downloaded locally and then the mailmerger.zip will be generated

<strong>Step 3</strong> - Inside mailmerger-jobs-webconsole implement the screens described in the above User Interface Guidelines section; mailmerger-jobs-webconsole should be a normal gradle sub-project

<strong>Step 4</strong> - Inside mailmerger-assembly/build.gradle implement the script to generate the needed mailmerger.zip file

![image](https://user-images.githubusercontent.com/19224635/40277989-265bc878-5c29-11e8-863c-7c0d0ad96678.png)




