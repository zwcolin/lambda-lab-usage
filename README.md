# lambda-lab-usage

## Setup account, create storage & instance 
1. Sign up or login to your account [here](https://lambdalabs.com/cloud/login). You may be asked to add a bank card in order to access the cloud.
2. Click on `storage` on the left panel, and then click `create filesystem`. Now you will have 10TB of persistent storage for free.
3. Click on `instances` on the left panel, then click `launch instance`. When you select instance, **please make sure to only select ones that are with A6000 or V100 GPU**. As these are only instances that can access the shared storage.
4. After you select the instance, you will see `Attach a filesystem` in the bottom. Make sure to select the storage that you created in step 2. Meanwhile, you may be asked to set up a SSH key. You will use it later if you want to access the terminal or port from local, but each instance will give you a web UI in a jupyterlab form, where you can call terminal, access files, etc on your browser. Note, if you want to transfer files, you need to use ssh. A tutorial for setting is up is [here](https://lambdalabs.com/blog/getting-started-with-lambda-cloud-gpu-instances/).
5. Click launch and agree to the agreements prompted later. The booting will take roughly 2 minutes. After that, you will be able to access the instance.

## Setup environment in each instance
*Note: in this section, I will be using the web UI to do that*
1. In the GPU Instances list, find the instance running and click `Launch` under Cloud IDE column.
2. Click `Terminal` under the `Other` section. Now, you might or might not see an error saying "Unexpected end of JSON Input". If you do and you cannot find the terminal you launched, click `Dismiss`, then click the **second** icon in the left panel, and after that click the terminal you just launched but failed to appear. That terminal should now appear.
3. (Only first time) If this is the first instance you have launched, cd into your persistent storage folder, then do `wget https://repo.anaconda.com/archive/Anaconda3-2020.02-Linux-x86_64.sh`.
4. Do `cd /home/ubuntu/<STORAGE_NAME> && bash Anaconda3-2020.02-Linux-x86_64.sh` to install Anaconda. Init the anaconda after installation, and close and relaunch a terminal following step 2 in this section.
5. Correct the GCC/G++ version. First, do `sudo apt -y install gcc-7 g++-7 gcc-8 g++-8 gcc-9 g++-9`. Then, do `sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 100` and `sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-7 100`.
6. Create an environment `conda create -n <NAME> python=3.7`. Then, activate it by `conda activate <NAME>`. **After this step, make sure to export the pip to PATH: `export PATH=~/anaconda3/envs/<NAME>/bin:$PATH`**. You might want to install pytorch first: `conda install pytorch==1.8.1 torchvision==0.9.1 torchaudio==0.8.1 cudatoolkit=11.1 -c pytorch -c conda-forge`.
7. Install all the necessary packages remain uninstalled. Here, I will use [EfficientZero](https://github.com/YeWR/EfficientZero) as an example, under python 3.7, torch 1.8.1, torchvisio n0.9.1, torchaudio 0.8.1 and cudatoolkit 11.1.
8. Do `git clone <CODEBASE>` or alike to transfer your codebase to your instance. If you transfer it to the persistent storage, then that codebase will be shared with all instances. Storing outside that storage folder will make the codebase only accessible/readable/writable to your current instance.
9. (Note) while installing gym[atari,roms,accept-rom-license]==0.15.7 it says that this version has no roms and accept-rom-license, which made me unable to train/test the model. I used the following [reference](https://github.com/openai/atari-py#roms) to manually fix it. Note: you only need to do 9.1-9.3 the first time you create the instance. For later instance setup, you can proceed to 9.4 directly.
9.1 Make a new directory (inside the root directory of your persistent storage) named `ROM`, then cd into it.
9.2 Download the ROM 2600 into the folder `wget http://www.atarimania.com/roms/Roms.rar`
9.3 Do `sudo apt install unrar` to install a software for unzipping. Then, do `sudo unrar e Roms.rar`
9.4 After all the games are unrar-ed, do `python -m atari_py.import_roms /home/ubuntu/experiment/rom`. This will copy-paste all the games into the atari-py package and you will see commands printing out after execution.
10. Now run with the command they provided `python main.py --env BreakoutNoFrameskip-v4 --case atari --opr train --amp_type torch_amp --num_gpus 1 --num_cpus 10 --cpu_actor 1 --gpu_actor 1 --force`. It will run. Note: you may notice things like `OSError: [Errno 22] Invalid argument: '/home/ubuntu/experiment/EfficientZero/results/atari/none/BreakoutNoFrameskip-v4/seed=0/Sun Aug 21 00:32:40 2022'`. I'm not sure why it happens, but after I change the log directory by removing the suspicious characters, it trains and evaluates successfully.

## Data transfer
*This is the official [tutorial](https://lambdalabs.com/blog/downloading-data-sets-lambda-cloud/) for transferring data between your local machine and the storage you have.*

Note: If you haven't set up ssh, you may want to set it up first.

TL'DR: Data transfer can be accomplished by simply using scp with your ssh key between cloud and local.
