# Mininet-sec Usage Documentation

[Mininet-sec](https://github.com/mininet-sec/mininet-sec#mininet-sec) is an emulation platform for studying and experimenting cybersecurity tools in programmable networks.

## Installation 

```
git clone https://github.com/mininet-sec/mininet-sec
cd mininet-sec
python3.9 -m pip install .
```

In the official documentation, the last command was "python3 -m pip install .", however, I executed it and got the error response "No module named pip", which was solved after executing "python3.9 -m pip install ." instead.

After executing this last command, I received this following error response, despite the fact that mininet was already installed in my system:

```
Processing /home/mayara/mininet-sec
  Preparing metadata (setup.py) ... error
  error: subprocess-exited-with-error
  
  × python setup.py egg_info did not run successfully.
  │ exit code: 1
  ╰─> [8 lines of output]
      Traceback (most recent call last):
        File "<string>", line 2, in <module>
        File "<pip-setuptools-caller>", line 34, in <module>
        File "/home/mayara/mininet-sec/setup.py", line 11, in <module>
          from mnsec.net import VERSION
        File "/home/mayara/mininet-sec/mnsec/net.py", line 18, in <module>
          from mininet.net import Mininet, MininetWithControlNet
      ModuleNotFoundError: No module named 'mininet'
      [end of output]
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
error: metadata-generation-failed

× Encountered error while generating package metadata.
╰─> See above for output.

```

The error was solved after the execution of the following commands:

```
sudo -i
cd ~
git clone https://github.com/mininet/mininet.git
export PYTHONPATH=$PYTHONPATH:$HOME/mininet
```

However, another error appeared:

```
Processing /home/mayara/mininet-sec
  Preparing metadata (setup.py) ... error
  error: subprocess-exited-with-error
  
  × python setup.py egg_info did not run successfully.
  │ exit code: 1
  ╰─> [1 lines of output]
      Cannot find required module honeypots.
      [end of output]
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
error: metadata-generation-failed

× Encountered error while generating package metadata.
╰─> See above for output.
```

