% CGI Programming
% Ravikiran K.S.
% January 1, 2006

## CGI

Common Gateway Interface mechanism has been standardized on most Web servers. In Document root of Web server, you create a subdirectory named cgi-bin. Any file requested from the cgi-bin directory isn't simply read and sent, but instead would be executed. Output of the executed program is sent to browser.

HelloWorld.html:
<html>
  <body>
     <h1>Hello World!</h1>
  </body>
</html>

HelloWorld.c:
#include <stdio.h>
int main()
{
  printf("Content-type: text/html\n\n");
  printf("<html>\n");
  printf("<body>\n");
  printf("<h1>Hello World!</h1>\n");
  printf("</body>\n");
  printf("</html>\n");
  return 0;
}

gcc helloworld.c -o helloworld.cgi		//Place helloworld.cgi in cgi-bin directory.

HelloWorld.pl:
#! /usr/bin/perl
print "Content-type: text/html\n\n";
print "<html><body><h1>Hello World!";
print "</h1></body></html>\n";

chmod 755 helloworld.pl					//Place helloworld.cgi in cgi-bin directory.

"Content-type: text/html\n\n" must be the first thing sent to the browser by any CGI script.

For information to be persistent, the data must be stored in some file/database. This is because program will exit soon that it has printed the information. Next time when program is run, if it wants to remember its last time state, it needs to preserve that data somewhere. Since data could be accessed by more than one user at same time, the program should be re-entrant and so should be the data stored on machine.

Sending input to a cgi-bin script can be done via URL. Ex.
http://xyz.abc/cgi-bin/pqr.cgi?param1=val&param2=value&param3=value&&param4=value&param5=value

<html>
<body>
  <h1>A super-simple form<h1>
  <FORM METHOD=GET ACTION="http://www.howstuffworks.com/cgi-bin/simpleform.cgi">
  Enter Your Name:
  <input name="Name" size=20 maxlength=50>
  <P>
  <INPUT TYPE=submit value="Submit">
  <INPUT TYPE=reset value="Reset">
  </FORM>
</body>
</html>

#include <stdio.h>
#include <stdlib.h>
int main()
{
  printf("Content-type: text/html\n\n");
  printf("<html>\n");
  printf("<body>\n");
  printf("<h1>The value entered was: ")
  printf("%s</h1>\n", getenv("QUERY_STRING"));
  printf("</body>\n");
  printf("</html>\n");
  return 0;
}
gcc simpleform.c -o simpleform.cgi

The value provided on URL if contains spaces, it should be replaced with a +. Ex. "John Smith" is given as "John+Smith"
http://xyz.abc/cgi-bin/pqr.cgi?name=John+Smith

Couple of checklists:
* Each input field on the form should have a unique identifier. That param-name/input-name should be unique for each field.
* The form needs to use GET or POST method.
	* With GET form values can be seen on URL, but not with POST.
	* With GET debugging is easier than POST.
	* GET has an upper limit on MAX number of characters that could be transmitted. POST is useful for large forms.
	* Query using GET is updated at QUERY_STRING environment variable. Use getenv()
	* Query using POST is available via stdin. Use gets() in C or read in PERL
	* On concat of all inputs into a single string, characters will be substituted, like space with +. So, translation required.
* Different environment variables available in CGI scripting - AUTH_TYPE, CONTENT_LENGTH, CONTENT_TYPE, GATEWAY_INTERFACE, HTTP_ACCEPT, HTTP_USER_AGENT, PATH_INFO, PATH_TRANSLATED, QUERY_STRING, REMOTE_ADDR, REMOTE_HOST, REMOTE_IDENT, REMOTE_USER, REQUEST_METHOD, SCRIPT_NAME, SERVER_NAME, SERVER_PORT, SERVER_PROTOCOL, SERVER_SOFTWARE

Multiple Form inputs:
* Single-line text input
Enter Name: <input name="Name" size=30 maxlength=50 value="Sample">
* Multi-line text input
<textarea name="Company Address" cols=30 rows=4></textarea>
* Check boxes
<input type=checkbox name="Include" value=1>
* Selection lists
Select an Option<br>
<SELECT size=2 NAME="Option">
    <OPTION> Option 1
    <OPTION> Option 2
    <OPTION> Option 3
    <OPTION> Option 4
</SELECT>
* Radio buttons
Choose the search area:<br>
<input type=radio CHECKED name=universe value=US-STOCK> Stocks
<input type=radio name=universe value=CA-STOCK> Canadian Stocks
<input type=radio name=universe value=MMF> Money Markets
<input type=radio name=universe value=MUTUAL> Mutual Funds
* Specialized buttons for submitting or clearing the form
<INPUT TYPE=submit value="Submit">
<INPUT TYPE=reset value="Reset">

Survey.html:
<html>
  <body>
    <h1>HSW Survey Form<h1>
    <FORM METHOD=POST ACTION="http://www.howstuffworks.com/cgi-bin/survey.cgi">
    Enter Your Name:
    <input name="Name" size=20 maxlength=50>
    <P>Enter your sex:
    <input type=radio CHECKED name=sex value=MALE>Male
    <input type=radio name=sex value=FEMALE>Female
    <P>Select your age<br>
    <SELECT size=2 NAME=age>
      <OPTION> 1-10
      <OPTION> 11-20
      <OPTION> 21-30
      <OPTION> 31-40
      <OPTION> 41-50
      <OPTION> 51-60
      <OPTION> 61 and up
    </SELECT>
    <P>Enter Your Comment:
    <input name="Name" size=40 maxlength=100>
    <P>
    <INPUT TYPE=submit value="Submit">
    <INPUT TYPE=reset value="Reset">
    </FORM>
  </body>
</html>

Redirection using CGI:
CGI scripts can be used to redirect the incoming request to some other page. To do that, the cgi binary/script should emit:
Location: http://www.nexturl.here

URL Encoding/Decoding a String:
Encoding is simple, you must replace certain special characters with their hex codes or + for space. Hex codes are prefixed with a % sign.

Cookies:
Cookies can be used to store some persistent data with client/browser. Next time when client visits server, it provides this information to server. Cookies sent to server by client are stored in HTTP_COOKIE environment variable. To set a cookie, you should emit something like below:
Set-Cookie: name=value; expires=date; path=path; domain=domain; secure

