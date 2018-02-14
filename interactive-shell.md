# Interactive shell

When Rubik’s Futuro Cube is connected to PC, it is possible to communicate with the cube using interactive command shell.

Shell can easily accessed from Rubik's Futuro Cube Suite, but is available for other applications as well, beacuse USB communication on cube provides common **VIRTUAL COM PORT**.

Before accessing the shell from other applications it is recommended to cycle USB to reset communication on character basis. This eventually resets more complex communication scheme   Note that in this case, characters are not automatically sent back to the terminal. Use command `echoon` if needed.



## 

## Setting shell to default from any app

If you want create a smart app that will communicate with RFC any time, there is a sequence that resets the shell to its default state under any condition \(no need to cycle USB\).  
The code is in python, but can be ported to any language you use.

```
##########################################
# somewhere in code open sline serial port
##########################################
sline = serial.Serial()
##########################################
def init_basic_shell(kill_clean = 1):
  sline.timeout = 0

  if (kill_clean):
    #"stdshell" in multiplex mode, see below  
    cmd = "SQ\x00\0x0a\x00stdshell\x0d\x70\x71\x00\x73"
  else:
    #"stdshellnk" in mulitplex mode, see below
    cmd = "\x53\x51\x00\x0c\x00\stdshellnk\x0d\x49\x71\x00\x73"

  sline.write(cmd)
  # we have to send once "\r" for case the multiplex was off
  # and we need to discard nonsense commands to obtain the prompt
  sline.write("\r")
  time.sleep(0.2)
  sline.flushInput()

  ##############################################################
  # at this moment, the cube should be communicating in a simple
  # shell with prompt ON and echo OFF
  ##############################################################
  # stdshell kills all ongoing apps
  # if you do not want to interfere
  # and let running apps continue,
  # use "stdshellnk" instead (kill_clean=0)
  ##############################################################
  # if you don't kill a running script on cube, the script can 
  # interfere with the shell with its output - 
  # use only if you know what you are doing :-)
  ##############################################################
```



