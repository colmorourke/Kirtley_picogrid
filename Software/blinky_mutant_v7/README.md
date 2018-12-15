# About BlinkyMutant_v7
- The goal of blinky mutant is to blink an LED on the Central controller via clicking a button on a LabView VI.
- Blinky Mutant is a modified version of Pohsu's Central Controller code.
- Originally, Matt Overlin & Colm were trying to modify the TI blinky example, to achieve our goal above.
- It proved to be easier, to simply modify Pohsu's code - the generation of blinky code created this way are now labelled "mutant".

## How to Flash this code?
To flash the code we use CCS Uniflash, as opposed to the debug mode in CCS.  
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
## Folder Structure
Describe file structure
- Labview folder has labview files
- There are 2 folders that remain untouched
F2837xD common & headers
