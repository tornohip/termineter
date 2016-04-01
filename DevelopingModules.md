# Developing Modules #
Modules consist of the core functionality of the framework.  The framework is designed to make developing modules easy by providing robust and easy to use modules for interacting with the meter.  Most of the code is documented for developing purposes and most classes, functions and class methods are documented with descriptions.

Once a module has been created all that there is to do is add it to the framwork/modules directory.  The name of the file will become the name of the module.  The restrictions on module names are that they can not start with a double underscore and the names must be all lower case.

# Module Layout #
Modules are class objects who's instances are created by the framework on initialization.  All modules must be derived from the `module_template` object and thus inherit all of it's methods.  The module is broken down into two main methods, the [init](DevelopingModules#init.md) method and the [run](DevelopingModules#run.md) method.  Although currently, only the init and run methods are used by the framework other methods may be defined.

## init ##
This method defines the modules properties and options. The possible attributes that can be defined are in the following table:
| Module Attribute | Type | Description |
|:-----------------|:-----|:------------|
| version          | Integer | The modules version number |
| author           | List of Strings | The names of the authors who made the module |
| description      | String | The modules short description, this shows up when 'show modules' is executed |
| detailed\_description | String | The modules long description, this shows up when 'info' is executed |

In addition to these attributes which can be set, the module comes with two option container instances, "options" and "advanced\_options".  To add user-configurable options to the modules, these two option container instances should be used.

Furthermore it is import for compatibility that the arguments that are passed to the modules init method are passed into the parent class's init method.  As such the first three lines of modules are:
```
class Module(module_template):
	def __init__(self, *args, **kwargs):
		module_template.__init__(self, *args, **kwargs)
```

## run ##
The run method contains the code for the task that the module is meant to carry out.  In addition to the 'self' parameter that all class methods include the run method is passed the framework instance.  The definition for the run method is: <pre>run(self, frmwk)</pre>

**Deprecation Notice:** Revisions prior to [r29](http://code.google.com/p/termineter/source/detail?r=a20b6a9f9ce1cacf1b5bf63e4059eebe2f92c0dc) also passed a list containing arguments to the module and has since been removed.

# Connecting to the Meter #
Prior to the module's run method being invoked the Termineter framework will ensure that the framework's serial connection is up and the meter is responding to basic requests.  If the module requires that the connection be authenticated then the framework's serial\_login method needs to be invoked. This will use the authentication settings set from within the framework.  Whether the serial connection is authenticated or not the raw C1218Connection instance can be accessed from the framework's serial\_connection attribute.  Modules can and should use the serial\_connection attribute from the framework to interface with meters.

# Displaying and Printing Information #
Modules should not use the print statement.  The framework instance provides two different ways to print information to the console.  These are the preferred methods of presenting information to the user.  In the future if there should be a new interface provided such as a GUI, it is important that the print statement not be used.  The two acceptable ways to print information are the framework print methods and the logging methods.
## Framework Print Methods ##
The framework instance (which is passed to modules) has 5 methods for displaying information.
| Method | Description |
|:-------|:------------|
| print\_error | Print error messages, starts with `[-]` |
| print\_status | Print any status messages, starts with `[*]` |
| print\_good | Print any message about a success, starts with `[+]` |
| print\_line | Print a generic message |
| print\_hexdump | Print binary data in formatted hex |

## Logging Methods ##
Modules also have a 'logger' attribute which can be referenced as `self.logger` which is a standard Python [logging.Logger](http://docs.python.org/2/library/logging.html) instance.  This logger is suitable for printing any optional information to the user.  The level of this logger is user configurable so not all messages will be shown to the user.  Developers should use this logger object for any applicable debugging messages.

**Deprecation Notice:** Revisions prior to [r30](http://code.google.com/p/termineter/source/detail?r=cb53fbc34b5570e3a158270d9fabf6929a58e2ce) used the framework get\_module\_logger method.  Although some modules still use this, future modules will be developed using the logger attribute.

# Tips #
While writing modules, developers can use the framework's 'reload' command to reload the module that they are currently working on.  This can allow them to execute an updated run method without restarting the framework.