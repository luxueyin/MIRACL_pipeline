# MIRACL_pipeline
Step-by-step documentation of the MIRACL pipeline setup on our local machine. Please refer to the pipeline published by the original Authors for more details: https://miracl.readthedocs.io/en/latest/index.html

## Step 1: Downloading Docker and Ubuntu

We will first download Docker Desktop from the Microsoft Store if the App was not already available. The version of Docker currently used is v29.1.5. By default, Docker Desktop is installed at C:\Program Files\Docker\Docker.

Download Ubuntu from the Microsoft Store. The current version used is 24.04.1 LTS release. 

1. Open Docker Desktop and navigate to Settings. In the General tab check if Use the WSL 2 based engine checkbox is checked. Check it if it isn't.
2. Still in the Settings navigate to Resources>WSL integration. Enable the Ubuntu distribution that you want to use Docker with.
3. Go back to the command prompt and open the Docker enabled Ubuntu distro in a new tab.
4. In the Linux terminal, type docker run hello to check if Docker is working correctly.

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

Troubleshooting the ```./install.sh``` command:









