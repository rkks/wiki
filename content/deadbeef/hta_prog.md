% HTA Programming
% Ravikiran K.S.
% January 1, 2006

If i get your question right , the closest thing to get what you want would be using *.hta which is a HTML Application that runs outside the browser window just like a normal app.

<script type="text/javascript" language="javascript">
    function RunFile() {
    WshShell = new ActiveXObject("WScript.Shell");
    WshShell.Run("c:/windows/system32/notepad.exe", 1, false);
    }
</script>


<html>
<head>
    <title>Application Executer</title>
    <HTA:APPLICATION ID="oMyApp" 
        APPLICATIONNAME="Application Executer" 
        BORDER="no"
        CAPTION="no"
        SHOWINTASKBAR="yes"
        SINGLEINSTANCE="yes"
        SYSMENU="yes"
        SCROLL="no"
        WINDOWSTATE="normal">
    <script type="text/javascript" language="javascript">
        function RunFile() {
        WshShell = new ActiveXObject("WScript.Shell");
        WshShell.Run("c:/windows/system32/notepad.exe", 1, false);
        }
    </script>
</head>
<body>
    <input type="button" value="Run Notepad" onclick="RunFile();"/>
</body>
</html>

<html>
    <head>
        <script type="text/javascript">
        function runProgram()
        {
            var shell = new ActiveXObject("WScript.Shell");                 
            var appWinMerge = "\"C:\\Program Files\\WinMerge\\WinMergeU.exe\" /e /s /u /wl /wr /maximize";
            var fileLeft = "\"D:\\Path\\to\\your\\file\"";
            var fileRight= "\"D:\\Path\\to\\your\\file2\"";
            shell.Run(appWinMerge + " " + fileLeft + " " + fileRight);
        }
        </script>
    </head>
    <body>
        <a href="javascript:runProgram()">Run program</a>
    </body>
</html>
