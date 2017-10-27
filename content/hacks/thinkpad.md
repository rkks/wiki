% Thinkpad Tips Tricks
% Ravikiran K.S.
% January 1, 2006

## Save some Space:

In Program Files\Lenovo\System Update\session you must keep System (subfolder),
Temp (subfolder), QuestResponse (xml document), Updates.ser (ser file) OR else
System Update won't start. You can safely delete all other directories.

Most of the files inside the WinSxS folder are hardlinks. To get the real size
of your Windows folder, use this tool:
http://www.heise.de/software/download/cttruesize/50272
The WinSxS\Backup folder is part of the Windows Resource Protection . NEVER
touch it in any way or you will break your Windows 7.

cleanmgr.exe (Disk Cleanup) -> System Files -> More -> System Restore & Shadow
Copy Delete

Internet Options -> History -> Change default space of 250MB to 50MB.

## Performance Improvements

Uninstall Thinkvantage Toolbox
Uninstall Chrome
Goto Task Scheduler - Microsoft -> Windows -> Maintenance -> Disable the Wisat

Enable Virtualization in BIOS:
Press F1 to enter BIOS. By Default AMD/VT is disabled in BIOS. Enable it under Security/Virtualization
