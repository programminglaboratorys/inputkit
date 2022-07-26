# inputkit
library of input functions. and an input API

# installation

Run the following to install:
```cmd
pip install inputkit
```
### or
```cmd
python -m pip install inputkit
```
if that didn't work, try replacing `pip` with `pip3`.

need help? or have bugs to report, my discord: [lab](https://discord.gg/vzEZnC7CM8)

# Welcome to the inputkit wiki!
#### [github wiki](https://github.com/programminglaboratorys/inputkit/wiki)
### library of input functions. and an input API
that can be used easily.

that can be helpful when getting inputs from the users


> ## pages
- [codes](https://github.com/programminglaboratorys/inputkit/wiki/CODES)
- [inputlib module](https://github.com/programminglaboratorys/inputkit/wiki/inputlib-module)
- [inputs module](https://github.com/programminglaboratorys/inputkit/wiki/inputs-module)
- [inputstools module](https://github.com/programminglaboratorys/inputkit/wiki/inputstools-module)

# codes
codes to copy ;)
## import everything
```python
import inputkit.inputstools as inputstools # some functions that i made for testing&fun
import inputkit.inputs as inputs
print("inputstools attrs",dir(inputstools))
print("inputs attrs",dir(inputs))
from inputkit import *
```

## those wait for a char/key lkinput
```python
key,char = any_input () # wait for user input. returns ((char|key) or together)
char     = char_input() # wait for user input. returns char
key      = key_input () # wait for user input. returns key
```

## hide a password
```python
password = password_input("Enter your password: ") # hide the input with (star="*")
```

## eval an input
```python
type = amazing_input("enter a type: ") # returns a type "10"=>int("10")
```

## parse an input to commandline list
```python
cmd,laststr = commandline_input("enter a command>") # parser the command returns list,laststr "hello \"hi i am a big string\" more stuff" => (['hello', 'hi i am a big string', 'more', 'stuff'], None)
```

## parser url
```python
parserUrl,querys = urlparse_input("enter a url: ") # string to ParseUrl "https://google.com/kit?hi=1&yay=omg" => (ParseResult(s
netloc='google.com', path='/kit', params='', query='hi=1&yay=omg', fragment=''), {'hi': ['1'], 'yay': ['omg']})
```

## wait for a type
```python
# going to change the default check function
def check(i,x,y,z): # x is the input value
	if x.__class__.__name__== "int": # check if the type is int by name
		i.stop() # i = Input(); stop(the loop)
num = input_wait_for("int","enter a number",check=check) # wait for input type int,list,dict,etc  
```

## check input
```python
def check(cls,input):
	input = input.lower()
	if input=="hello":
		print("hello")
	elif input=="yo":
		print("yo, bro")
	elif input=="good":
		print("me too")
	else:
		print("whats up?")
		return "no input" # return before we stop the input
	cls.stop() # stop the input loop
	return "bye"
output = check_input(prompt,check=check) # check_input(prompt="",check=None,input=input); (input = input) default input function
print(output) # output: bye
```

## limit input length
```python
limited_string = limit_input("prompt:",length=14) # Limit an input length return when the len(input) is equal to length
```

## child process
```python
# porcess keyword is a function. that can be replaced
proc = subprocess_input("enter process command:") # run a child process. warpper for commandline_input
"""
another example subprocess_input(prompt="",process=None,*args,**kw):
	>>> subprocess_input("enter process command:",timeout=10,stdout=-1,process=subprocess.run)
	enter process command:python -c "print('Hello')"
	CompletedProcess(args=['python', '-c', "print('Hello')"], returncode=0, stdout=b'Hello\r\n')
"""
```

## os.system input
```python
code = system_input("enter a command:") # run a commandline command returns the exit code
```

## ignore input
```
ignore_input() # a just input to ignore any inputs. help ("""ignore_input will wait for enter or ^C""")
```

## yes or no input
```python
f_t = yn_input("Do you want to eat some vegetables?(y/n):") # warpper for check_input """ yes|no input wait for the user to input ([yes,y,true] or [no,n,false]) returns booling"""
"""
another example yn_input(prompt=""):
	>>> yn_input("Do you want to eat some vegetables?(y/n):")
	Do you want to eat some vegetables?(y/n):what
	invalid option for (y/n). try again
	Do you want to eat some vegetables?(y/n):no
	False # returns False cuz i enter no
"""
```
## Input class
Input API
```python
from Input import *
from inputstools import parser_keys
# how to create attr 'proxy_string'
with Input("Hello World!:",proxy_string=[],extra_events=dict()) as _input:
	_input.add_event("on_newline",built_in_events_Input.on_newline)
	@_input.event
	def on_update(cls,start="\x0D",deleted=False):
		if deleted: # going to delete char from the prompt
			try:
				cls.proxy_string[len(cls)] = " " # replace it with
				print(start+cls.prompt+"".join(cls.proxy_string),end="")
				del cls.proxy_string[len(cls)] # deleted the space after printing it
			except IndexError: # handler
				cls.proxy_string = list(cls) # replace cls.proxy_string with the input
		print(start+cls.prompt+"".join(cls.proxy_string),end="")

	@_input.event
	def on_write(cls,module,prompt):
		cls.dispatch("update") # create an event
	@_input.event
	def on_start(cls):
		cls.run() # start running
	@_input.event
	def on_read(cls,k,c):
		if c == b"\r": # pressed enter or ^M
			cls.stop() # stop running
		elif k == b"\x08": # on press BackSpace
			cls-=1
			cls.dispatch("update",deleted=True) # create an event
		elif k == b'\xe0' or k == b"\x00": # type ignore
			#key = parser_keys(c.decode())  # get KeyError if the key is not defind
			#print(key,"has been prassed")
			# Parser(UP = "H", DOWN = "P", RIGHT = "M", LEFT = "K")
			pass # we're going to ignore this.
		else:
			cls+=c.decode() # add char to cls.input
			cls.proxy_string=list(cls)
	@_input.event
	def on_close(cls,output):
		print(cls.proxy_string)

	output = _input.start()
	print()
	print(repr(output)) # the input
```
> **Note**
> cls.input type of String not str.
> str doesn't support -=
