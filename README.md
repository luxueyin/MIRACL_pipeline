# MIRACL_pipeline
Step-by-step documentation of the MIRACL pipeline setup on our local machine. Please refer to the pipeline published by the original Authors for more details: https://miracl.readthedocs.io/en/latest/index.html

## Command Cheatsheet
Find out where you are: ```pwd```
Find out what user account you are currently in: ```whoami```
Go back to home directory: ```cd ~```
Go back one directory: ```cd ..```
Go into a directory: ```cd [folder name]```
Show list of items inside current directory: ```ls```

To make a new folder: ```mkdir [folder name]```
To delete a directory: ```rm -r [folder name]```
To delete a file: ``` rm [file name]```


## Step 1: Downloading Docker and Ubuntu

We will first download Docker Desktop from the Microsoft Store if the App was not already available. The version of Docker currently used is v29.1.5. By default, Docker Desktop is installed at C:\Program Files\Docker\Docker.

Download Ubuntu from the Microsoft Store. The current version used is 24.04.1 LTS release. 

1. Open Docker Desktop and navigate to Settings. In the General tab check if Use the WSL 2 based engine checkbox is checked. Check it if it isn't.
2. Still in the Settings navigate to Resources>WSL integration. Enable the Ubuntu distribution that you want to use Docker with.
3. Go back to the command prompt and open the Docker enabled Ubuntu distro in a new tab.
4. In the Linux terminal, type docker run hello to check if Docker is working correctly.

It is recommended to completely close Ubuntu and Docker Desktop and relaunch to ensure WSL2 Ubuntu is running. You can also double check this using the PowerShell Terminal and type in ```wsl --status``` to check if WSL2 Ubuntu is selected and use code ```wsl -l -v``` to confirm if Docker Desktop and Ubuntu is currently running.


## Step 2: Cloning MIRACL into a local drive
In the WSL2 Ubuntu terminal make a folder in the desired location to clone the MIRACL repo to your machine:
```
git clone https://www.github.com/mgoubran/MIRACL
```

Change into the newly created directory where you cloned MIRACL to:
```
cd MIRACL
```

Build the latest MIRACL image using the installation script we provide:
```
./install.sh
```

### Troubleshooting the ```./install.sh``` command:

1. In order for the ```./install.sh``` script to work, Docker should NOT be used with ```sudo```. Our script checks and exits if it is being run with ```sudo``` priviledges. The reason for this behavior is that the installation script creates a user in the Docker container that matches the ```uid``` and ```gid``` of the host user which is required correct X11 forwarding. This user should NOT be ```root``` which is the case when Docker commands are executed with ```sudo```. For more information on how to add a docker user to use Docker without ```sudo``` visit the official Docker documentation.

2. Make sure that the script can be executed. If it can’t and you are the owner of the file, use ```chmod u+x install.sh``` to make it executable. Prefix the chmod command with ```sudo``` if you are not the owner of the file or change permissions for ```g``` and/or ```o```.

3. Issue with no login name
This can happen when WSL is trying to fetch logname but there is no logname present. To begin troubleshooting type in ```whoami``` and it should return your user name and in our case ```choforcelab```. This can be fixed by manually editing the ```install.sh``` file and replace ```logname``` with ```whoami```. The place containing ```logname``` should look something like ```USER_NAME=$(logname)``` which should be replaced with ```USER_NAME=$(whoami)```. Save and exit the file then run ```./install.sh``` again.

After running the command ```./install.sh``` without any flags will start an interactive installation process that will run you through an abbreviated version of the installation.

```
$ ./install.sh
No flags provided, starting interactive prompt...
Enter Docker image name (default: 'miracl_mgoubran_kirk_4918_img'):
Enter Docker container name (default: 'miracl_mgoubran_kirk_4918'):
Enable GPU in Docker container (required for ACE) (y/N): y
Enter the location of your data on your host system (default: None). If you choose a location, your data will be mounted at '/data' in your container: /data5/projects/
```
Press enter for teh image and container prompts to choose the default names or enter your preferred names. It's recommended to enter your own name such as ```miracl``` so it's easier to remember when entering the container in the future. GPU forwarding will be disabled by defaults so enter ```y``` at the prompt to enable it. Lastly, you will be prompted for the lcoation of yoru data on the host system. The default will be on the C: drive. Please provide the full path to your location. For reference MIRACL container is mounted under ```/mnt/d/cholab/MIRACL```. Entering the wrong location will result in deleting the container and recreating the entire MIRACL container again.


## Step 3: Entering docker container
Once the image has successfully been built, run the container using **Docker Compose**:
```
docker compose up -d
```

To enter your container:
```
docker exec -it <CONTAINER_NAME> bash
```

To exit your container and navtigate to your MIRACL folder:
```
exit
```


## Step 4: Checks
To make sure that you set the GPU option during installation. Once the installation is complete, enter the ```docker``` continer using ```docker exec -it <CONTAINER_NAME> bash``` and run the ```nvidia-smi``` command to ensure your GPU is detected.

The pre-trained DL models are publically available and will have automatically been downloaded when you installed MIRACL

Once you have the models, place them in the required directory structure by using the following commands:
```
cp <PATH TO UNET MODEL FILE> <WHERE YOU CLONED MIRACL>/miracl/seg/models/unet/best_metric_model.pth
cp <PATH TO UNETR MODEL FILE> <WHERE YOU CLONED MIRACL>/miracl/seg/models/unetr/best_metric_model.pth
```

To check that the models are in the correct directory structure, run the following commands:
```
ls <WHERE YOU CLONED MIRACL>/miracl/seg/models/unet/best_metric_model.pth
ls <WHERE YOU CLONED MIRACL>/miracl/seg/models/unetr/best_metric_model.pth
```

The output should be similar to the following:
```
best_metric_model.pth
best_metric_model.pth
```


## Step 5: Download sample data

To test the MIRACL pipeline, download the sample data:
```
docker exec -it <CONTAINER_NAME> bash
cd <WHERE YOU WANT TO DOWNLOAD DATA>
download_sample_data
```

For our purposes, we have downloaded Mode 2 data and retrived the entire file after the zip file was done downloading.

## Data analyses (Segementation and Registration Combined)

AI-based Cartography of Ensembles (ACE) pipeline highlights:

- 1. Cutting-edge vision transformer and CNN-based DL architectures trained on very large LSFM datasets (refer to example section) to map brain-wide local/laminar neuronal activity.
- 2. Optimized cluster-wise statistical analysis with a threshold-free enhancement approach to chart subpopulation-specific effects at the laminar and local level, without restricting the analysis to atlas-defined regions (refer to example section).
- 3. Modules for providing DL model uncertainty estimates and fine-tuning.
- 4. Interface with MIRACL registration.
- 5. Ability to map the connectivity between clusters of activations.









