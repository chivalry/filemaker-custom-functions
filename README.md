# FileMaker Custom Functions

These are the standard custom functions that I generally import into all of my FileMaker
projects. They represent a library of custom functions, some commonly used, others more
esoteric.

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
which case it is only every called by the wrapper function, then the recursive tool is
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
    // Purpose:	description
    // Parameters:	  _param_1: description
    //              	_param_2: description
    //
    // Requirements: requirements
    //
    // Author:		Charles Ross
    // Version:	  1.0 written 15-03-07
    //
    // Notes:		Notes
    //
    // Example:
    // sample = result

The template is what would normall show between FileMaker's parameter list and the custom
function code. For example, if there was a function actually called template and it
actually took two parameters as above, the template line would have `Template ( _param_1;
_param_2 )`. If there's not parameters for the function, the template should not have
parenthesis.

The example portion should be something that, once the custom function has been defined,
would allow a developer to copy and paste into the data viewer, remove the commenting and
have it return `True`. An example, using one of FileMaker's built-in functions, might be
`Middle ( "abc"; 2; 1 ) = "a"`. If multiple examples are included, use a single
calculation that uses a boolean `and` to create the truth statement. If the test is
trivial because a constant is being returned, this can be omitted.

If the function depends on any other custom functions, these dependencies should be
documented in the requirements section.
