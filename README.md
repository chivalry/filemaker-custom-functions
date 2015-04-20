# FileMaker Custom Functions

These are the standard custom functions that I generally import into all of my FileMaker
projects. They represent a library of custom functions, some commonly used, others more
esoteric, but each one has actually been used or seems to have a high liklihood of
utility in some future project.

While I wrote many of these myself, there are quite a few that are from other sources.
I've tried to acknowledge the original author when I could as well as provide a URL to
the web page where the function was originally found. If you find a function that you
wrote without an attribution to you, please open an issue so I can fix that, or, if the
text format is available for it, fork the repository, make your change and submit a pull
request.

## Acknowledgements

Many of these functions are from sources other than myself. I've tried to include attributions when I knew who originally wrote them, but this information is sometimes missing. If you know who wrote a particular function for which an attribution is missing (see the issues for some authors that I'm looking for), please let me know. Among the FileMaker developers that I can definitively thank for their contributions to the FileMaker community (in no particular order):

- Matt Petrowski
- Jeremy Bante
- Winfried Huslik
- Matt Wills
- Kevin Frank
- Tom Robinson
- Jesse Antunes
- Agnès Barouh
- Arnold Kegebein
- Geoff Coffey
- Andrew Persons
- Theo Gantos
- Mikhail Edoshin
- The Shadow
- Vaughan Bromfield
- Bob Weaver
- LaRetta
- Fabrice Nordman
- Joe Scarpetta
- James Scarpetta
- Will M. Baker
- Brad Lowry
- Alexander Zueiv
- Jesse Swensen
- Debi Fuchs

## Formats

All of the custom functions can be found in three places:

1. Within a FileMaker 13 file, `Main.fmp12`. This is the golden master and the only place a standard function is guaranteed to be.
2. Within a FileMaker 11 file, `Main.fp7`. Incompatible custom functions that take advantage of FileMaker 12+ features, such as `ExecuteSQL`, will not be included here. Functions here may not yet have been updated to the latest version, but exist to ease importing into FileMaker systems based on versions 11-. Eventually it may cease to receive any updates.
3. Within a text file in a parent folder named for the functions group, such as `Main/Lists/lsts.First.fmcalc`. The `fmcalc` extension will allow the proper syntax highlighting within vim if the [filemaker.vim](https://github.com/chivalry/filemaker.vim) plugin is installed. These files are included for two reasons: to show line by line differences as functions are updated and provide the ability to retrieve the code for a single function. Note that if you copy and paste the code from a text file, you'll want to make sure you also manualy create any custom functions it depends upon.

There's one more file included for historical reference only, `Archive.fmp12`,
which contains the custom functions I had in their original format before I began editing
them to conform to standards and eliminating them if they weren't actually used or likely
to be used. It also contains functions that were created for specific applications,
unlikely to be generally useful as they are, but worthy of retention for some other reason,
perhaps as a reference to the algorithm used. If a function is collected but not used in
main because it's not yet a standard function, it may be stored here.

## Standards

These custom functions strive to conform to the following standards. Not all do yet, but
I hope to correct the shortcomings in the near future.

### Dividers

These custom functions are divided into groups. Since FileMaker doesn't (yet) offer
folders within the custom function dialog box, the grouping occurs with two kludges.
First, there is one custom function for each group that acts solely as a heading. Second,
each group is signified with a four-letter prefix unique to the group.

Headers begin with the four-letter prefix, followed by five underscores, the group name,
appended with underscores to provide a sufficient dividing line when the Function Name
column of the custom function dialog is widened to a fair width. This leads to custom
function headers named like the following:

    appl_____ Application Functions ___________________________________
    devp_____ Developer _______________________________________________
    lsts_____ Lists ___________________________________________________

Divider functions also contain comments on the purpose of the group.

### Normal Functions

Normal custom functions start with the four-letter prefix, followed by a dot, and then a
descriptive name in `CamelCase`, which is how FileMaker's built-in functions are named.
Some examples follow.

    date.BusinessDays
    devp.IsDeveloper
    dict.Add
    lsts.RemoveDuplicates

### Wrapped Recursive Functions

If a function's sole purpose is to act as a recursive tool for a wrapper function, in
which case it is only ever called by the wrapper function, then the recursive tool is
named identically to the wrapper function but with an appended underscore.

    lsts.Concat
    lsts.Concat_
    prtf.PortalFilterCreator
    prtf.PortalFilterCreator_

### Parameters

Parameters are named in `snake_case`, but with a leading underscore, as in `_list` and
`_folder_path`.  

### Preamble

Each custom function starts with a preamble that looks like the following:

    // Temmplate
    //
    // Purpose:	     description
    //
    // Parameters:	 _param: description
    //
    // Requirements: requirements
    //
    // Author:		 Charles Ross
    // Version:	     1.0 written 15-03-07
    //
    // Notes:		 Notes
    //
    // Example:
    // sample = result

The template is what would normally show between FileMaker's parameter list and the custom
function code. For example, if there was a function actually called Template and it
actually took two parameters as above, the template line would have `Template ( _param_1 )`.
If there're no parameters for the function, the template should not have parentheses.

The example portion should be a sort of unit test for the function, something that, once
the custom function has been defined, would allow a developer to copy and paste into the
data viewer, remove the commenting and have it return `True`. An example, using one of
FileMaker's built-in functions, might be `Middle ( "abc"; 2; 1 ) = "a"`. If multiple
examples are included, use a single calculation that uses a boolean `and` to create the
truth statement. If the test is trivial because a constant is being returned, this can
be omitted.

Such unit tests aren't always possible, expecially when the funciton's primary purposes
resides not in its return value but in its side effects. If possible, use comments in the
example to explain what the initial conditions should be and use the example to not only
express the return value of the function but also what side effects should also be true
after calling it.

If the function depends on any other custom functions, these dependencies should be
documented in the requirements section.

### Script Variables

If script variables are used within a custom function to track information between recursive
calls, the script variable names should include the custom function to reduce the chance
that custom function script variables will conflict with those found in actual executing
scripts. For example, if a function is named `MyCustomFunction` and it references a script
variable that under normal circumstances might be named `$_len`, with the custom funciton
a name of `$_MyCustomFunction_len`, or perhaps `$_mcf_len`, should be used. While this can
unfortunately result in very long script variable names within custom functions, using this
namespacing technique is preferable to having a script reference the same variable and use
the custom function in question, thereby overwriting the script's variable value.
