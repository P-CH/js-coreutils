# Coreutils

Created by P-CH. 2023

Disclaimer: If you run into any understanding issues, make sure to hit me up... (Discord: P-CH.#4795)

This is a JavaScript utility package designed to make a programmers life a bit easier. The module contains:
- Parser
- File Designator Grabber
- Function that exits the program after leaving a message
- Colored Text Printer (for CLI)
- Difference Calculator for two numbers
- Stdin reader
- Generator for standard set of characters (ASCII 32 through 126)

## Import Utilities

You can import parts of the module in different ways, here two examples:

```js
let {Parser, FileD} = require("./coreutils/main.js"); //imports Parser and FileD
let CliCP = require("./coreutils/main.js").CliColorPrint; //imports CliColorPrint as CliCP
```

## Usage

### Parser

Firstly, create an instance of the class:

```js
let parser = new Parser(env);
```
where ``env`` should be the environment you want to parse, for example ``process.argv`` to parse from the command line.

#### Help

You can set the help message to be displayed when calling ``showhelp()`` like this:

```js
parser.help = "your message here";
```

If you want to include information about what parameters can be used, just add ``<options>`` in the string. "<options>" itself will be replaced with the actual information when ``showhelp()`` is called.

The function ``showhelp()`` takes two optional arguments:
1. a message (default: what ``parser.help`` is set to)
2. a boolean whether the application should terminate after sending the help message (default: true)

```js
parser.showhelp(); //displays the help message and exits
parser.showhelp("extra information"); //displays "extra information" and exits
parser.showhelp("something", false); //displays "something" and doesn't exit
```

#### Actual Config

There are two functions available - ``set()`` and ``add()``. The set function will overwrite all existing settings (not the help message though) where as the add function just adds one or more parser elements.
Both functions take the same kind of object as argument:

```js
obj = {
    name: {
        call: "-x",
        desc: "does something",
        type: "regex" || "bool" || "toggle",
        check: true,
        default: undefined;
    },
    name2: {
        call: "-y",
        desc: "does something too",
        type: "regex" || "bool" || "toggle",
        check: 1 + 1 == 2,
        default: "nope";
    }
}
```

```js
parser.set(obj);
parser.add(obj2);
```

The name of individual objects defines by what name it'll be accessable in other funtions.
Here a list of what all the objects keys do:

|key|functionality|
|---|-------------|
|call|the parameter that has to be present in the CLI invocation in order to "activate" the parser object|
|desc|basically just the explanation for the parser object which is displayed in the help message|
|type|defines how the argument after the call is going to be checked for its validity: "regex" for a regex check, "bool" for a boolean equation and "toggle" if there doesn't need to be another argument|
|check|regex pattern if the type is "regex", a boolean equation for "bool" or just "true" if the type is "toggle"|
|default|the value that is returned if the check fails|

Please make sure to only use "regex", "bool" or "toggle" as type, otherwise all checks will result in false. All properties must be set, otherwise the application will throw an error...

#### Get values

To get the value of a parser object, run the ``get()`` function with the name of the parser object as argument.

```js
parser.get("name");
```

There is also a debug function called ``show()`` to see the config of a certain parser object. You have to supply the name of the parser object in order to get the entire object. If you want to get a certain property of it, pass the name of the property as second argument.

```js
parser.show("name"); //returns the entire parser object
parser.show("name", "check"); //returns the "check" property value of the "name" parser object
```

### FileD


This function will get the file designator of a file. Files with special file designators are usually found on UNIX systems, Windows does not have most of them.
The ``FileD()`` function takes an absolute path of file to be checked as the argument.

```js
FileD("path");
```

The following table explains the return values:

|return value|file type|
|------------|---------|
|d|directory|
|l|symbolic link|
|p|named pipe|
|s|socket|
|b|block device|
|c|character device|
|-|"normal" file|
|?|unknown (insufficient read permissions)|

### ExitMsg

Leaves a message in the CLI and then terminates the application. The first argument is the message to be displayed, the second is optional and defines the exit code to be used (set to 0 by default).

```js
ExitMsg("bye bye");
ExitMsg("bye bye", 1);
```

### CliColorPrint

This function allows to print colored text in the CLI without the user having to know what the escaped character codes for the colors are.

```js
CliColorPrint("<!fgred>something failed!<!fgwhite>");
```

The function replaces all tags in the following format:
"<!fgcolor>" or "<!bgcolor>"

The "fg" and "bg" stand for "foreground" and "background" and therefore define if the fore- or background color is changed. Available colors:
- black
- red
- green
- yellow
- blue
- magenta
- cyan
- white


### Diff

This function takes in two numeric values and returns the difference between them.

```js
Diff(102, -4) //returns 106
```


### Stdin

Getting a users input in JavaScript is not all that pleasant, Stdin allows you to do just that easily.

```js
async function main(){
    let input = await Stdin();
    console.log(input);
}
```


### Characters

The Characters variable contains an array of the ASCII characters with DEC 32 to 126 (so " " to "~")

```js
Characters[0] //returns the space character
```