---
layout: post
title: "About import inheritance"
description: python imports
tags: python import inheritance oop
date: 2017-12-12
---


I recently stumbled into an issue that had me stumped for a couple days.  
I have been writing a small [tool](https://github.com/sk1u/domain-google-calendar) that would read a list of domains, their registrar, and their date of expiry,
and would take that data to create a Google Calendar reminder for each domain.  

It's relatively simple to do, knowing how to get Python to read csv files, loop into them, and interfacing with the Google API, for which there is [excellent documentation](https://developers.google.com/api-client-library/python/start/get_started).

This is one of various automation tools that I have been putting together, to make my life easier both at GiG and for my webhosting adventures.

Anyway, the program is divided into two parts, the first being the *google\_calendar\_toolkit.py*, which has first of all the google authentication flow, and secondly a (very small, for now) collection of functions which will be executing various tasks related to Google Calendar, for me to comfortably call in my other programs.  

The second part, is the *bulk\_event\_adder*, the main portion of the program, which takes the CSV file as its input and has the program logic built in it, importing the *add\_reminder()* func from *google\_calendar\_toolkit*.

My goal was to be able to run something like the following:  
`python bulk_event_adder.py list.csv`  

And have it work its magic that way.  
I would have done that in the simplest way possible, by starting with:  
```
from google_calendar_toolkit import add_domain_expiry_event
import csv
import time
import sys

def main():
with open(sys.argv[1], 'rb') as csvfile:
    reader = csv.reader(csvfile, delimiter=',', quotechar='|')
    for row in reader:
        do_stuff()
```

`sys.argv[1]` being the desired filename.  

Of course trying to run this via:  
`python bulk_event_adder.py list.csv`  

yielded a weird error:
```
usage: bulk_event_adder.py [-h] [--auth_host_name AUTH_HOST_NAME]
                       [--noauth_local_webserver]
                       [--auth_host_port [AUTH_HOST_PORT [AUTH_HOST_PORT ...]]]
                       [--logging_level {DEBUG,INFO,WARNING,ERROR,CRITICAL}]
bulk_event_adder.py: error: unrecognized arguments: list.csv
```

Wat.  
I've used similar code before and it always worked, like clockwork.


The program was _NOT_ behaving the way I intended, plus it was throwing in an error which i would expect from argparse.
Some time after scouring [Stack Overflow](https://stackoverflow.com/), it hit me: my code was fine. A code snippet used in *google\_calendar\_toolkit* was the culprit:  

```
try:
    import argparse
    flags = argparse.ArgumentParser(parents=[tools.argparser]).parse_args()
except ImportError:
    flags = None
```

This is defined nearly at the beginning of the file, and used as part of the oauth2 flow for authenticating to Google.

My main *bulk\_event\_adder* was infact inheriting that import, and behaving as if i had imported and initialized argparse.

This is because when we write:  
`import some_module`

We really are writing something like:  
`some_module = import_module("some_module")`  

Where `import_module` is kind of like:  
```
def import_module(modname):
    if modname in sys.modules:
        module = sys.modules[modname]
    else:
        filename = find_file_for_module(modname)
        python_code = open(filename).read()
        module = create_module_from_code(python_code)
        sys.modules[modname] = module
    return module
```

_In short: If the name has been imported elsewhere, it will be in the global list of all modules, and so will be reused._

Workarounds to this?
Initialising `argparse` in the imported module, under
```
if __name__ == '__main__':
    ...
```
at the bottom of the program.  
This way, Argparse is initialised only if the imported module is run as main.

Of course it is to be seen if argparse needs to be initialised outside of that, depending on the context.
In the case of my *domain-google-calendar*, I simply chose to call the csv by its filename in the script, so that it can be run as follows:  
`python bulk_event_adder.py`

