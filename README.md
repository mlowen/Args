# Args

Args is a micro framework for command line applications, it creates the needed parameters that should be passed to the application based on the message signatures that it is supplied.

### Inspiration

The inspiration for Args came from wanting to have a command driven cli app similar to how git works which I was unable to replicate with [argparse](http://docs.python.org/dev/library/argparse.html).

## Installation

Currently the only way to use args is to download it and add it to your project. In the future once there is some tests setup and the naming and conventions of the framework is finalised then the setup will be modified to use distribute and possibly upload to [pypi](https://pypi.python.org/pypi). 

## Documentation

### arguments.Runner

#### Runner(description = None, show_usage = True, prefix = '--')
The constructor for the Runner class, the parameters usage are as follows:

* *description* - The description of the application which is displayed on the help screen.
* *show_usage* - When this is set to `False` then the help screen is not shown when an unknown command is received.
* *prefix* - This is the prefix characters for the optional parameters and flags.

#### Runner[command_name] = handler

This is how assign a command to be run, the `command_name` is the string that when passed to the application identifies what handler to run.  This expects either a function or a tuple which contains a function and a description, the description from the tuple is used on the help screen.

#### Runner.run(args = None)

When run is called it parses the arguments and calls the appropriate handler. When `args` is `None` then the command line arguments are pulled from `sys.argv`

### arguments.utils

**is_none_or_empty(obj)** - Takes a string or list and returns true if the the object passed in is either None or has a length of 0.

**is_none_or_whitespace(string)** - Takes a string and returns a boolean, true if the argument is None or composed of only whitespace characters.

### Argument types

There are three types of arguments that are generated by arg which are based on the signature of the method that is passed to args, the 

* **Positional** - A positional parameter is generated by an argument on the a method which does not have a default value. To specify a positional parameter only the value of the parameter needs to specified
* **Optional** - An optional parameters is generated by an argument on a method which has a default value which is not a boolean. To invoke an optional parameter from the command line the parameter name needs to specified with the prefix followed by the value for the parameter.
* **Flags** - A flag is defined is generated by an argument on a method which has  a default value that is `False`. To invoke a flag from the command line all that is needed are the prefix characters and the flag name.

### Built-in functionality

Part of the functionality built into args is the ability to generate help screen for running the application and a help screen for each individual command.  When generating the help screen for a command args will use any parameter annotations that are available for the help text.

## Example

A basic hello world example 

**test.py**

```python
import arguments

def default():
	print('This is the default argument.')

def hello(name:'The name of who to say hello to.'):
	print('Hello %s.' % name)

def goodbye(name = None, niceday = False):
	msg = 'Good bye'
	
	if name is not None:
		msg += ' %s' % name
	
	if niceday:
		msg += ', have a nice day.'
	
	print(msg)

runner = arguments.Runner()

runner[None] = default
runner['hello'] = (hello, 'Says hello and exits.')
runner['goodbye'] = (goodbye, 'Says goodbye and exits')

if __name__ == '__main__':
	runner.run()
```

### Output

**Command:** `./test.py`
**Output:** 
```
This is the default action
```

**Command:** `./test.py hello foo`
**Output:** 
```
Hello foo.
```

**Command:** `./test.py goodbye`
**Output:** 
```
Good bye
```

**Command:** `./test.py --name foo`
**Output:** 
```
Good bye foo
```

**Command:** `./test.py --name foo`
**Output:** 
```
Good bye, have a nice day.
```

**Command:** `./test.py --name foo`
**Output:** 
```
Good bye foo, have a nice day.
```

**Command:** `./test.py help`
**Output:**
```
usage: ./test.py [<command>]

Available commands:
  hello     Says hello and exits.
  goodbye   Says goodbye and exits
  help      Show this message and exit

See './test.py help --command <command>' for help on that command.
```

## Future Plans

* Finalise naming.
* Tests.
* Uploading to pypi.
* Type inference of arguments.
* Command overloading.

## License

Args is available under the MIT license which is as follows:

Copyright © 2013 Michael Lowen

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.