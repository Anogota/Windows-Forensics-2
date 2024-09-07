# Windows-Forensics-2

Hello to everyone, we made room Windows-Forensics, should be great, yesterday i made same room but version 1 off it.
I tought i will find something intresting stuff :)


<h3>1.The FAT file systems</h3>

Task 1:How many addressable bits are there in the FAT32 file system?
FAT32, the actual bits used to address clusters are 28, so the maximum number of clusters

Task 2:What is the maximum file size supported by the FAT32 file system?
FAT32, the maximum volume size and the maximum file size (4GB - 1 file size for both FAT16 and FAT32)

Task 3:Which file system is used by digital cameras and SD cards?
The exFAT file system is now the default for SD cards larger than 32GB.


<h3>2.The NTFS File System</h3>
Run CMD as Administrator, then navigate to C:\Users\THM-4n6\Desktop\Eztools, you will see there that tool: MFTECmd.exe. Run that program, you will see the following options:

```
MFTECmd version 0.5.0.1

Author: Eric Zimmerman (saericzimmerman@gmail.com)
https://github.com/EricZimmerman/MFTECmd

        f               File to process ($MFT | $J | $LogFile | $Boot | $SDS). Required
        m               $MFT file to use when -f points to a $J file (Use this to resolve parent path in $J CSV output).

        json            Directory to save JSON formatted results to. This or --csv required unless --de or --body is specified
        jsonf           File name to save JSON formatted results to. When present, overrides default name
        csv             Directory to save CSV formatted results to. This or --json required unless --de or --body is specified
        csvf            File name to save CSV formatted results to. When present, overrides default name

        body            Directory to save bodyfile formatted results to. --bdl is also required when using this option
        bodyf           File name to save body formatted results to. When present, overrides default name
        bdl             Drive letter (C, D, etc.) to use with bodyfile. Only the drive letter itself should be provided
        blf             When true, use LF vs CRLF for newlines. Default is FALSE

        dd              Directory to save exported FILE record. --do is also required when using this option
        do              Offset of the FILE record to dump as decimal or hex. Ex: 5120 or 0x1400 Use --de or --vl 1 to see offsets

        de              Dump full details for entry/sequence #. Format is 'Entry' or 'Entry-Seq' as decimal or hex. Example: 5, 624-5 or 0x270-0x5.
        fls             When true, displays contents of directory specified by --de. Ignored when --de points to a file.
        ds              Dump full details for Security Id as decimal or hex. Example: 624 or 0x270

        dt              The custom date/time format to use when displaying time stamps. Default is: yyyy-MM-dd HH:mm:ss.fffffff
        sn              Include DOS file name types. Default is FALSE
        fl              Generate condensed file listing. Requires --csv. Default is FALSE
        at              When true, include all timestamps from 0x30 attribute vs only when they differ from 0x10. Default is FALSE

        vss             Process all Volume Shadow Copies that exist on drive specified by -f . Default is FALSE
        dedupe          Deduplicate -f & VSCs based on SHA-1. First file found wins. Default is FALSE

        debug           Show debug information during processing
        trace           Show trace information during processing


Examples: MFTECmd.exe -f "C:\Temp\SomeMFT" --csv "c:\temp\out" --csvf MyOutputFile.csv
          MFTECmd.exe -f "C:\Temp\SomeMFT" --csv "c:\temp\out"
          MFTECmd.exe -f "C:\Temp\SomeMFT" --json "c:\temp\jsonout"
          MFTECmd.exe -f "C:\Temp\SomeMFT" --body "c:\temp\bout" --bdl c
          MFTECmd.exe -f "C:\Temp\SomeMFT" --de 5-5

          Short options (single letter) are prefixed with a single dash. Long commands are prefixed with two dashes

-f is required. Exiting
```

For parsing the $MFT file, we can use the following command:  ```MFTECmd.exe -f <path-to-$MFT-file> --csv <path-to-save-results-in-csv>```
You can then use the EZviewer tool inside the EZtools folder to view the output of MFTECmd, or to view CSV files

Task 1:Parse the $MFT file placed in C:\users\THM-4n6\Desktop\triage\C\ and analyze it. What is the Size of the file located at .\Windows\Security\logs\SceSetupLog.etl

I used that command: ```MFTECmd.exe -f C:\users\THM-4n6\Desktop\triage\C\$MFT --csv C:\Users\THM-4n6\Desktop``` that fill .csv will save on our Desktop, if everything its okay you should see that:
CSV output will be saved to 'C:\Users\THM-4n6\Desktop\20240907114013_MFTECmd_$MFT_Output.csv'

Navigate on Desktop to EZtools\EZviewer and then run that program EZviewer, than open that csv in program. and you will see something what's look like this:

![image](https://github.com/user-attachments/assets/49089639-77ec-4620-9837-97b6f5ec097d)

Task 1:Parse the $MFT file placed in C:\users\THM-4n6\Desktop\triage\C\ and analyze it. What is the Size of the file located at .\Windows\Security\logs\SceSetupLog.etl
Press CTRL+F and search SceSetupLog.etl you should see this: 

![image](https://github.com/user-attachments/assets/792edd7d-4a96-48ee-8d51-83e84ae42592)

The answer is: 49152

Task 2:What is the size of the cluster for the volume from which this triage was taken?
I checked the hint, because i had no idea what should i do :D
Hint: Parse the $Boot file. If you are having trouble viewing the CSV file, you can use EZviewer from the EZtools folder

So i parse the $Boot, like in previouse example.

![image](https://github.com/user-attachments/assets/5f3b1592-b201-429c-9e35-b49432764399)

And you can find answer in Index Entry Size: 4096


<h3>3.Recovering deleted files</h3>
First what we need to do is nagivate on Desktop you will there a icon Autopsy, just clik it :P
Click on New Case:

![image](https://github.com/user-attachments/assets/fd4f1434-715c-47f7-9667-7a6cf20b9e54)

Enter a name to save your case by, and click Next.
You can add the required details here. For now, we can click Finish to move forward. Autopsy will perform some processing and then show the following screen. Click Next to move forward.
You will see this screen. Since we will be performing analysis on a disk image, select the topmost option, Disk Image or VM File.
Provide the location of the data source. You will find a disk image named 'usb.001' on the Desktop. Provide the path to that file in the above window and click next. You will see the following window:
click Deselect All. These are different modules that Autopsy runs on the data for processing. For this task, we don't need any of these. If enabled, they take a lot of time to run. Click Next after clicking Deselect All. Autopsy will load the disk image. You will see the following in the left panel.

Then navigate to Deleted Files\File System 

![image](https://github.com/user-attachments/assets/4ef3eacc-59c7-43ed-8052-c1cbb5613087)

Task 1:There is another xlsx file that was deleted. What is the full name of that file?
Tryhackme.xlsx

Task 2:What is the name of the TXT file that was deleted from the disk?
TryHackMe2.txt

Task 3:Recover the TXT file from Question #2. What was written in this txt file?
Click on the TryHackMe2.txt then Extract Files to Desktop

![image](https://github.com/user-attachments/assets/517a9488-514c-42cf-918d-eaaf24d5ffc8)

<h3>4.Evidence of Execution</h3>

Task 1:How many times was gkape.exe executed?
Open CMD as Administor, navigate to ```C:\users\THM-4n6\Desktop\EZtools``` then run that command ```PECmd.exe -d C:\Users\THM-4n6\Desktop\triage\C\Windows\prefetch --csv C:\Users\THM-4n6\Desktop```

Then load that file into EZViewer

![image](https://github.com/user-attachments/assets/567b29f3-e152-46f1-9db3-139cbc7fcee1)

And the answer will be 2.

Task 2:What is the last execution time of gkape.exe
We need to run that command ```PECmd.exe -d C:\Users\THM-4n6\Desktop\triage\C\Windows\prefetch --csv C:\Users\THM-4n6\DesktopWxTCmd.exe C:\Users\THM-4n6\Desktop\triage\C\Users\THM-4n6\AppData\Local\ConnectedDevicesPlatform\L.THM-4n6\ActivitiesCache.db --csv C:\Users\THM-4n6\Desktop```

And do what's we did in previouse task, navigate to EZViewver and load that file
The answer will be: What is the last execution time of gkape.exe

Task 3:When Notepad.exe was opened on 11/30/2021 at 10:56, how long did it remain in focus?
```WxTCmd.exe -f C:\Users\THM-4n6\Desktop\triage\C\Users\THM-4n6\AppData\Local\ConnectedDevicesPlatform\L.THM-4n6\ActivitiesCache.db — csv C:\Users\THM-4n6\Desktop``` - we need to run that command and load into EZViewver, search by Notepad.exe and you will see duration: 00:00:41🕓

![image](https://github.com/user-attachments/assets/0c25b7fb-047c-4806-88be-cf93a7aa484f)
