# System wide custom xkb keyboard overlays
This little tool allows you to create an overlay (keys, strings, commands) for your
xkb keyboard.<br>
Specify a key (by default Caps Lock), disable its normal behaviour and use it as an activator for
another key layer on your keyboard.<br>
This also allows usage of navigation keys such as Up, Down, Left and Right on the home row.<br>
The new key layer can be customized using the JSON configuration file.

## Installation
- If you like to use command and string injection, please install python-xlib<br>
(if you do not have superuser privileges, you can also download it and compile it yourself)
<pre>$ sudo apt-get install python-xlib</pre>

- clone this repository to a path_of_your_choice.
<pre>$ git clone https://github.com/soeiner/homerow-arrowkeys.git path_of_your_choice</pre>

- change working directory to the installation directory
<pre>$ cd path_of_your_choice</pre>

- make scripts executable
<pre>$ chmod +x mainloop.py resume.py lib/xdotool/xdotool</pre>

- test it
<pre>$ ./mainloop.py &</pre>
now open an editor and try pressing some keys with and without caps held down.

- add it to start up (tested on Ubuntu based systems)<br>
Go to the launcher and open the program 'Startup Applications'.<br>Click on 'Add'.<br>Enter some name.<br>Click on 'Browse'.<br>Navigate to 'mainloop.py'.

- add it to resume directory so the overlays still work after resume
<pre>$ sudo cp resume.py /etc/pm/sleep.d/</pre>

## Configuration File
A default configuration file is already included. Add more mappings and functionality by editing it.<br>
There are three types of mapping:

### Cross Mapping
Map a key code directly to another key code.<br>
Example: In the default overlay, i is mapped directly to your Up key, for every application,
it will look as if you pressed the Up arrow key.
<pre>
...
{
  "key_code": 32,
  "mapped_key_label": "PGDN"
},
...
</pre>

### Key Symbol Mapping
Map a key code directly to another key code generated by the program. Specify a key symbol,
e.g. 'braceleft' and the program uses a previously unused key code to create a new 'virtual key'.
Then it creates a 'Cross Mapping'.
<pre>
...
{
  "key_code": 16,
  "mapped_keysym": "braceleft"
},
...
</pre>
(Key code 16 is mapped to 7, AltGr+7 creates braceleft on the de layout)<br>
Note that this method requires unused key codes in your keymap, which are limited.

### Command and String Mapping
This requires the installation of `python-xlib`.<br>
Map a key code to a set of commands, also including shell commands.<br>
Define two sequences of commands, one for key up and one for key down.<br>
A command can be a string (evaluated as shell command), or an object in JSON notation.<br>
This shows an example found in the default configuration, which allows you to execute the command
"fuck" (https://github.com/nvbn/thefuck) with just two key strokes ;)
<pre>
...
{
  "key_code": 41,
  "mapped_sequences": {
    "down": [
      {"text": "fuck"},
      {"key": "Return"}
    ]
}
...
</pre>
Note that this method requires unused key codes in your keymap, which are limited.<br>
This will be improved in the future.
