## kivy-ios cheatsheet

### instruction

source [link](https://www.youtube.com/watch?v=6gLGyrlgqCU)

see Development section in kivi-app github repo
[link](https://github.com/kivy/kivy-ios)

1. find path to your local dir with two project files `*.py` and `*.kv`. In
   this exaample path is:

```bash
~/PycharmProjects/kivy_mobile_timer/project
```

2. clone the repository from Development section and use all the described commands in the above
   sections. Clone and install it to your local virtual environment:

```bash
# clone the repo in sepatate dir and cd in it
git clone https://github.com/kivy/kivy-ios.git
cd kivy-ios/

# reate venv, activate it and install all components
python3 -m venv venv
. venv/bin/activate
pip install -e .

# In this venv
# install cython
pip install Cython

# build the kivy
python toolchain.py build kivy python3

# create XCode project
python toolchain.py create kivy_mobile_timer ~/PycharmProjects/kivy_mobile_timer/project
# it returns:
#Project directory : kivy_mobile_timer-ios
#XCode project     : kivy_mobile_timer-ios/kivy_mobile_timer.xcodeproj

# open the project in XCode
open kivy_mobile_timer-ios/kivy_mobile_timer.xcodeproj

```

if you need some required module just use

```bash
python toolchain.py pip install <name_of_module>
```

---

THE END
