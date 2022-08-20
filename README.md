# lambda-lab-usage

## Setup account, create storage & instance 
1. Sign up or login to your account [here](https://lambdalabs.com/cloud/login). You may be asked to add a bank card in order to access the cloud.
2. Click on `storage` on the left panel, and then click `create filesystem`. Now you will have 10TB of persistent storage for free.
3. Click on `instances` on the left panel, then click `launch instance`. When you select instance, **please make sure to only select ones that are with A6000 or V100 GPU**. As these are only instances that can access the shared storage.
4. After you select the instance, you will see `Attach a filesystem` in the bottom. Make sure to select the storage that you created in step 2. Meanwhile, you may be asked to set up a SSH key. You will use it later if you want to access the terminal or port from local, but each instance will give you a web UI in a jupyterlab form, where you can call terminal, access files, etc on your browser.
5. Click launch and agree to the agreements prompted later. The booting will take roughly 2 minutes. After that, you will be able to access the instance.

## Setup environment in each instance
*Note: in this section, I will be using the web UI to do that*
1. In the GPU Instances list, find the instance running and click `Launch` under Cloud IDE column.
2. Click `Terminal` under the `Other` section. Now, you might or might not see an error saying "Unexpected end of JSON Input". If you do and you cannot find the terminal you launched, click `Dismiss`, then click the **second** icon in the left panel, and after that click the terminal you just launched but failed to appear. That terminal should now appear.
3. (Only first time) If this is the first instance you have launched, cd into your persistent storage folder, then do `wget https://repo.anaconda.com/archive/Anaconda3-2020.02-Linux-x86_64.sh`.
4. Do `cd /home/ubuntu/<STORAGE_NAME> && bash Anaconda3-2020.02-Linux-x86_64.sh` to install Anaconda. Init the anaconda after installation, and close and relaunch a terminal following step 2 in this section.
5. Create an environment `conda create -n <NAME> python=3.7`. Then, activate it by `conda activate <NAME>`. You might want to install pytorch first: `conda install pytorch==1.8.1 torchvision==0.9.1 torchaudio==0.8.1 cudatoolkit=11.1 -c pytorch -c conda-forge`. (You can try to use pip for torch installation but I encounted many bugs in my early trials. Use `conda` takes a couple of minutes to find the packages but it'll make everything at ease).
6. Install all the necessary packages remain uninstalled. Here, I will use [EfficientZero](https://github.com/YeWR/EfficientZero) as an example, under python 3.7, torch 1.8.1, torchvisio n0.9.1, torchaudio 0.8.1 and cudatoolkit 11.1

## Data transfer

