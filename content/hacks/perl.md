## Programming

; - statement separator, same as C
All functions list can be found on perlfunc page

2 basic data types
* numbers - only digits with no comma or space in between
* strings - everything within single or double quotes. The difference between single and double quotes is:
	double quote - interpreted. That is \n is a new line symbol.
	single quote - taken literally. That is \n are two more characters.

3 types of variables
``
* scalars - Can be a number or string. Begins with $. Ex, $i = 5; $name = 'xyz';
* vectors/arrays - List of scalars. Begins with @. Ex. @a = (1,2,3,4,5); @months = ("jun","jul","aug"); As with C, index begins at 0.
* hashes - dictionaries/key-value pairs. Beings with %. Ex, %days_in_summer = ( "July" => 31, "August" => 31, "September" => 30 );
Note: All the above 3 types of variables have 3 different namespaces. Meaning that $abc is different from @abc, two different variables.
Similarly, $abc[0] is not same as $abc{0}. First is 0th index of array, second is Key with value 0 in hash table.
``

``
Numbers can be manipulated by arithmetic operations in same ways as in C. Ex,
$a = 5; # $a is a number. Variable type gets decided by assignment.
$a = $a + 1; OR $a += 1; OR $a++;
$a = $a - 1; OR $a -= 1; OR $a--;
$a = $a * 2; OR $a *= 2;
$a = $a / 2; OR $a /= 2;

Strings support concatenation operation by '.' operator.
$a = "hello"; # $a is a string. Double quotes are used.
$b = $a . "world"; # $b is now "hello world"

Perl converts strings to numbers transparently whenever it's needed. Hence, below operation is valid
$a = "8";
$b = $a + "1";	# $b will be 9

Any element of an array can be accessed by index similar to C as:
print "0th element is" $arr[0];
$arr[1] = "0";

Length of an array can be found using $#<array-name>. Ex, $#arr. This will always be 1 less than total number of elements in array.
If there are no (zero) elements in array, it will return -1.

To resize an array, just changing value of $#<array> would suffice. Ex,
@nums = (1, 2, 3, 4, 5, 6, 7, 8, 9); print "Number of elements in array is $#nums"
$#nums = 0; print "Number of elements in array is $#nums"

An array is also created when we try to assign a value to one of its elements. Ex.
$winter_months[0] = "December";  # This implicitly creates array @winter_months.
print "Array: $winter_months[0] Num Elements: $#winter_months\n";
Note: This can be bad, as a non-existing/mis-spelled array doesn't complain, rather gets created on its own.

Accessing hash values is done as $<hashname>{<key>}. Ex,
%ex_kv = ( "A" => 1, "B" => 2, "C" => 3 );
print $ex_kv{"A"}." ".$ex_kv{"B"}." ".$ex_kv{"C"}."\n";;

To see all keys in hash 'keys' function can be used with input of 'hashname'.
%ex_kv = ( "A" => 1, "B" => 2, "C" => 3 );
print keys %ex_kv;

Looping can be achieved by many functions, below are few:
* for - simiar to C, but here it is a function. Ex, for $i (1, 2, 3, 4, 5) { print "$i\n"; }

To specify range of numbers, '..' can be used. Ex,
@one_to_ten = (1 .. 10);
It can be extended to looping concepts too as:
$top_limit = 25; for $i (@one_to_ten, 15, 20 .. $top_limit) { print "$i\n"; }

For looping, the elements doesn't have to be numbers. strings can also be used easily. Ex.
for $i (keys %months) {
    print "$i has $months{$i} days.\n";
}
``

## Quick Tips Tricks

$ perl -e 'printf ("%d\n", $@)'

* Search and replace in all files under current directory
``
$ perl -pi -e 's/foo/bar/g' sb.*cfg
$ perl -pi -e 's/find/replace/g' *.txt
``

* Installing only perl modules as non-root using PREFIX/LIB/... envs just doesnt work
$ perl -MCPAN -e 'install Bundle::CPAN'

* To check for if any perl package is present on not, do:
$ perl -e 'use Foo::Bar::Baz;'
Can't locate Foo/Bar/Baz.pm in @INC (@INC contains: 
..............

If this error comes out, it doesnt exactly mean that this module isnt installed
on the current system, it just means that the module isn't in @INC include path.
There are different ways by which you can add your perl module in @INC include path
1. Specify it in PERL5LIB environment variable
    export PERL5LIB=path
2. Perl takes -I option where you can specify the additional directories where
it should search for modules.
    perl -I path myscript.pl
3. Modify your Perl program to find the module by adding below line near the top
of Perl programs. This simple line of code tells your Perl program to add this
directory to it's @INC search directory.
    use lib '/<path>';
PATH has got to be the absolute path. Then you can print the @INC as:
    print "\@INC is @INC\n";

* Some debug tips
perl -c hello
$ /usr/bin/perl -d
perl -w hello

* CPAN configuration and installation (non-root) !but didnt work.
$ mkdir ~/scripts/settings/cpan/CPAN
mkdir: created directory `/homes/raviks/scripts/settings/cpan'
mkdir: created directory `/homes/raviks/scripts/settings/cpan/CPAN'
$ echo "\$CPAN::Config = {}"> ~/scripts/settings/cpan/CPAN/MyConfig.pm
$ ln -s /homes/raviks/scripts/settings/cpan/ ~/.cpan
$ perl -MCPAN -we 'shell'
/homes/raviks/.cpan/CPAN/MyConfig.pm initialized.
Are you ready for manual configuration? [yes]
I see you already have a  directory
    /homes/raviks/.cpan
Shall we use it as the general CPAN build and cache directory?
CPAN build and cache directory? [/homes/raviks/.cpan]
Cache size for build directory (in MB)? [10]
Perform cache scanning (atstart or never)? [atstart]
Cache metadata (yes/no)? [yes]
Your terminal expects ISO-8859-1 (yes/no)? [yes]
File to save your history? [/homes/raviks/.cpan/histfile]
Number of lines to save? [100]
Policy on building prerequisites (follow, ask or ignore)? [ask]
What is your favorite shell? [/bin/tcsh] /bin/bash
    PREFIX=~/perl       non-root users (please see manual for more hints)
Your choice:  []~/tools/linux/perl
    -j3              dual processor system
Your choice:  [] -j3
    UNINST=1         to always uninstall potentially conflicting files
Your choice:  [-j3] UNINST=0
Timeout for inactivity during Makefile.PL? [0]
Select your continent (or several nearby continents) [] 2
Select your country (or several nearby countries) [] 2
commit: wrote /homes/raviks/.cpan/CPAN/MyConfig.pm

cpan> install Bundle::CPAN
LIB=~/tools/perl/lib INSTALLSITEMAN1DIR=~/tools/perl/man/man1 INSTALLSITEMAN3DIR=~/tools/perl/man/man3 UNINST=0
