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

I used that command: ```MFTECmd.exe -f C:\users\THM-4n6\Desktop\triage\C\$MFT --csv C:\Users\THM-4n6\Desktop``` that fill .csv will save on our Desktop





