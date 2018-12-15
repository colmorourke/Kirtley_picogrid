# Software Guide & Tips
Programming with TI's code composer studio (CCS) is certainly not intuitive.
One often has to follow a very specific set of sequential steps, that are not at all obvious.  
Most of what is talked about here relates to the TI Delfino (TMS320F28377D) microcontroller. However, Labview also has its quirks and these will also be included here.

### Goals of this Document
- Describe how the kirtley_picogrid software is organised and how to run it.
- Describe the tips we have learned along the way.
- Put all of this information into a single document.

## Table of Contents
Here is the list of topics we discuss:
- [Software Organisation Overview](#folder_structure)
- [Running blinky TI example on Deflino](#runblinky)
- [Copy & Paste TI CCS project](#ccs_copy)
- [Target Configuration Files](#targetconfig)
- Demos:
	- blinky_mutant_v7
		- This example blinks an LED on the Central Controller PCB via clicking a button on the Labview control panel.
- [Uniflash](#uniflash)

## <a name="folder_structure"></a> Overview of Software Organisation
Features of our approach:
- Self-Contained
	- Our goal is to have self-contained folders that run specific demos/projects. All software associated with a given demo should be located in the same place. 
	- This means we duplicate some files (eg TI libraries, Header files etc) for each new project. However, we prefer this approach than to require our projects to link to files in other folders outside that project folder.
- Typical Demo/Project Layout:
	- *NOTE!!!* This layout will probably be a little different in the future to cater for projects with multiple microcontrollers.
	- Labview folder contains LabView .VI file for that specific project
	- There are 2 folders originally obtained from TI examples, that remain untouched
		- F2837xD_common
		- F2837xD_headers
	- Comm & Peripheral Folders
		- These may be edited a little or a lot depending on the application
	- main_xxx.c
		- Usually unique to an application
	- [Target Configuration File](#targetconfig)
	- [Uniflash .OUT file](#uniflash)
		- All of our projects can only be exclusively flashed using Uniflash, except for TI examples which are flashed with the CCS debug. For some reason, I think this is only visible when browsing via CCS Uniflash. It probably has a different name in windows explorer.

## <a name="runblinky"></a>How to safely run & edit blinky on the Delfino?
Suppose you wanted to use the Blinkey example that TI provide in CCS.
How can you copy this project, and modify it, without modifying the original?  
Make sure these first 2 steps are already done:
1. Correctly [copy file  to workspace](#ccs_copy)
2. Setup [Target Configuration file](#targetconfig)  

Next, follow these steps:
- Right click project name
	- Build Project
- Right click on project name
	- Debug As--> Code Composer Debug Session
		- Click green triangle  
Enjoy LED blinkage!!

### <a name="ccs_copy"></a> How to Copy TI CCS projects?
This description is aimed at guiding those who want to copy and edit the TI example code. However, the steps used are the same for copying TI projects in general.

TI provide examples when you download CCS:
- C: ti/ControlSuite/device_support/F2837xD/v210/F2837xD_examples_Cpu1
- Or C: ti/ControlSuite/device_support/F2837xD/v210/F2837xD_examples_Dual

There are 2 methods to correctly copy. We reccommend method 1, but we may need to resort to method 2 at times:
1. Copy & Paste in CCS Environment
- Steps:
	- Open the project you wish to copy in CCS
	- Right click on project name, and then click copy.  
	- Then, paste project into the workspace
- *WARNING*:  When setting up CCS for the very first time, this method may not work.
	- Initially, Matt & Colm spent 4+ hours attempting this method when simply trying to get the basic blinky working, but couldn't get it to work.
	- We had an issue where all of the C files in the copied project were "linked files" as apposed to "C Source" files.
		- *NOTE:* Right click on main file in project and check properties to see if it's a linked file or not.
	- This meant that when we made changes to our copied project, the orignal project also changed.
- To solve this issue we were forced to initially follow method 2 described below.
	- This kept the properties of the main.c file etc as C source files
	- For every subsequent copy of the latest project, we were able to use the simple method 1 (copy & paste in CCS).

2. Copy & Paste In Windows Explorer
- Steps:
	- Open windows explorer:
		- Copy the project you wish to duplicate.
		- Paste this project in the desired windows explorer location.
	- Open CCS:
		- Best practise is to change name of original project in CCS.
			- If the original project is open in CCS, and has the same name as the one we are about to import to our workspace, then we will get an error.
		- In CCS: File-->Switch Workspace-->Other…
			- Browse to whatever folder in Windows you want to use. Eg MIT-Pico-Grid/Software
		- In CCS: File-->Import-->C/C++ --> CCS Projects. Click Next
			- A new window appears
			- Click Browse in order to select search directory 
			- Select folder where the project we wish to copy (eg TI example) is located
				- Recall  For TI examples:
					- C: ti/ControlSuite/device_support/F2837xD/v210/F2837xD_examples_Cpu1
					- Or C: ti/ControlSuite/device_support/F2837xD/v210/F2837xD_examples_Dual
			- Once you select either of above folders, a bunch of "discovered projects" will appear
			 	- Select the discovered project folder you wish to use
				- *NOTE:* Check boxes as follows (this may not matter that much...)
					- [x] Automatically import referenced projects found in search directory
					- [ ] Copy projects into workspace

###  <a name="targetconfig"></a> How to create a Target Configuration file
The target configuration file contains information on the DSP we're using, what programmer hardware we're using etc. The compiler needs to know this information to create the binary file output.
The steps to create this file are as follows:
- Open the project in CCS
- Right click project name
- New --> Target Config File
	- We need to provide 2 pieces of info:
		1. Connection:
			Check product number of device connecting USB to programmer.
			Ie "Texas Instruments XDS2XX USB Debug Probe"
		2. Board or Device:
			For us this is the TMS320F28377D
-  Lastly, we can actually test our config file
	- Double click the name of the ".ccxml" file
	- Click "Test Connection" Button
	- A window will appear and will tell us whether we succeed
	

## <a name="uniflash"></a> Uniflash
To flash our code we use CCS Uniflash, as opposed to the debug mode in CCS.  
*NOTE:* We don't actually know how to flash using CCS debug mode when trying to flash code derived from Pohsu's code!! Pohsu doesn't know how to do this either. Uniflash is faster anyway, so we have decided that this doesn't bother us for now.

Steps to use Uniflash:
1. Build Code in CCS.
	- This generates a .OUT file located in CPU1_flash folder.
2. Open Uniflash
	- Open Target Configuration-->Browse-->find location of .ccxml file
		- targetConfigs folder, file ending in 79D.ccxml
	- A Window will appear
		- Ignore all of the options - no need to change anything
		- Click Program-->Load Program-->Browse--> go to .OUT file located in CPU1_flash folder

