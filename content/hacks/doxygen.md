% Doxygen Tips Tricks
% Ravikiran K.S.
% January 1, 2006

Each project requires its own doxygen configuration file which can be created
using 'doxygen -g <conf-file-name>' command.

For small projects, leave INPUT tag empty. Doxygen then parses through current
directory to find source files. RECURSIVE tag needs to be set to 'yes' for
recursive find to happen.

However, for large projects assign the project root directory or sub directories
within to the INPUT tag. Also you can specify types of files to look for using
FILE_PATTERNS tag. EXCLUDE_PATTERNS tag can be used to specify directories to be
excluded.

EXTRACT_ALL can be set to 'yes' to get documentation for a project that may not
have doxygen style comments embedded.

Set SOURCE_BROWSER and INLINE_SOURCES to 'YES' for code cross-reference to be
generated.
