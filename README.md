# Jupyter-Kernels
Steps to setup multiple kernels with Jupyter

# Installing Jupyter
```
pip install jupyter
pip install  ipykernel
```
# Installing other kernels

## Bash
```
pip install bash_kernel
python -m bash_kernel.install
 ```

## Octave
```
sudo apt-get install octave
pip install octave_kernel
python -m octave_kernel.install
 ```
## Julia
Install julia by following steps in http://julialang.org/downloads/platform.html
for ubuntu run bellow commands
```
sudo add-apt-repository ppa:staticfloat/juliareleases
sudo add-apt-repository ppa:staticfloat/julia-deps
sudo apt-get update
sudo apt-get install julia
sudo apt-get install libnettle4
```
Run this in Julia
```julia
Pkg.add("IJulia")
```

## Scala
```
sudo apt-get install scala
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
export SCALA_HOME=/usr/local/share/scala
export PATH=$PATH:$SCALA_HOME/bin
export JAVA_HOME=/usr/lib/jvm/java-8-oracle
wget "https://oss.sonatype.org/content/repositories/snapshots/sh/jove/jove-scala-cli_2.11/0.1.1-1-SNAPSHOT/jove-scala-cli_2.11-0.1.1-1-SNAPSHOT.tar.gz"
tar -xvzf jove-scala-cli_2.11-0.1.1-1-SNAPSHOT.tar.gz
cd jove-scala-cli-0.1.1-1-SNAPSHOT/bin
./jove-scala --kernel-spec
```

## R
``` 
sudo add-apt-repository ppa:marutter/rrutter
sudo apt-get update
sudo apt-get install r-base r-base-dev
sudo apt-get install libzmq3-dev libcurl4-openssl-dev
```
Run this in R
```R
install.packages("devtools")
install.packages('RCurl')
library(devtools)
install_github('armstrtw/rzmq')
install_github('IRkernel/repr')
install_github('IRkernel/IRdisplay')
install_github('IRkernel/IRkernel')
IRkernel::installspec()
```

## Python 3
```
sudo apt-get install python3-pip
sudo pip3 install -U ipython
sudo pip3 install jupyter_client
sudo pip3 install ipykernel
sudo ipython3 kernelspec install-self
```

## To add any virtual environment
Activate your python virtual environment to be added before 
if virtual wraper present then
```
workon MyVEnviron
```
else
```
source ~/Envs/MyVEnviron/bin/activate
```
once the virtual environment is active run bellow code
```
pip install jupyter
python -m ipykernel install --user --name MyVEnviron --display-name "Python (MyVEnviron)"
deactivate
```
replace `MyVEnviron` with your virtual environment name

## Matlab
You must have Matlab installed on your system - Duke has a site licesne so you should be able to get it.
```
pip install pymatbridge
pip install matlab_kernel
python -m pymatbridge.install
python -m matlab_kernel.install
```

## Installing extensions
### Spell-checking
```
! ipython install-nbextension https://bitbucket.org/ipre/calico/downloads/calico-spell-check-1.0.zip
```
### Notebook sections
```
! ipython install-nbextension https://bitbucket.org/ipre/calico/downloads/calico-document-tools-1.0.zip
```
## To configure slide view in Jupyter notebook
```
git clone https://github.com/ahmadfaizalbh/RISE.git
cd RISE
python setup.py install
```

## To setup you Jupyter notebook in server 
genrate configuration file
```
jupyter notebook --generate-config
```
#### Using SSL for encrypted communication
genrate self signed ssl cirtificate
```
openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mykey.key -out mycert.pem
```

#### Preparing a hashed password
run this in jupyter console
```python
In [1]: from notebook.auth import passwd
In [2]: passwd()
Enter password:
Verify password:
Out[2]: 'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed'
```
Edit config file (`~/.jupyter/jupyter_notebook_config.py`) and uncomment bellow mentioned 
lines and add appropriate value
```python
c = get_config()
c.IPKernalApp.pylab = 'inline'
# Set options for certfile, ip, password, and toggle off browser auto-opening
c.NotebookApp.certfile = u'/absolute/path/to/your/certificate/mycert.pem'
c.NotebookApp.keyfile = u'/absolute/path/to/your/certificate/mykey.key'
# Set ip to '*' to bind on all interfaces (ips) for the public server
c.NotebookApp.ip = '*'
c.NotebookApp.notebook_dir = u'/home/pynb/pynb'
c.NotebookApp.open_browser = False
c.NotebookApp.password = u'sha1:bcd259ccf...<your hashed password here>'
c.NotebookApp.port = 9999
```

now every thing is configured run `jupyter notebook` to start notebook in server


## To Set `jupyter notebook` as Background process
do any one of bellow and restart your server 
#### To put it in background and run as root
 >  add bellow lines to `/etc/rc.local`
 >  ```
  export JAVA_HOME=/usr/lib/jvm/java-8-oracle
  jupyter  notebook
  exit 0
  ```

#### To put it in background and run as some other user (USER) with less permisiion
 >  add bellow lines to `/etc/rc.local` also remember to change `USER`with appropriate username
 >  ```
  su USER -c 'export JAVA_HOME=/usr/lib/jvm/java-8-oracle;jupyter  notebook'
  exit 0
  ```

#### To put it in background and run as some other user with less permisiion and a seperate virtual environment
> set up vertual environment inside the user (`USER`) directry and install `Jupyter` inside this environment
> add bellow lines to `/etc/rc.local` also remember to change `USER`with appropriate username
>  in bellow case I have asumed that your environments in folder `/home/USER/Envs` and your environment name is `MyVEnviron` please change these appropriately
 >  ```
  su USER -c 'export JAVA_HOME=/usr/lib/jvm/java-8-oracle;export WORKON_HOME=/home/USER/Envs;source /home/USER/Envs/MyVEnviron/bin/activate;/home/USER/Envs/MyVEnviron/bin/jupyter  notebook;deactivate'
  exit 0
  ```
once server restarts wait for a minute and then try in your browser [https://your server IP:9999]() , your browser may show security warning please go to advance options and accept certificate. and then refresh your page it will prompt for password, enter the one used to genrate hashed password.

# Done!
