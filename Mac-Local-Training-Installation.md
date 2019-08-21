All steps are based on:
- crr0004's [Local Training](https://github.com/crr0004/deepracer)
- joezen777's revised [OSX Specific Steps](https://gist.github.com/joezen777/6657bbe2bd4add5d1cdbd44db9761edb)

Credit goes to them for doing all of the hard work!

#### Bold lines are terminal commands

## Preparation

1. In a terminal window, redirect to ~/ if need be
2. If Homebrew isn't installed: 
- **/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"**
3. **brew install minio/stable/minio**
4. Install [VNCViewer](https://www.realvnc.com/download/file/viewer.files/VNC-Viewer-6.19.325-MacOSX-x86_64.dmg)
5. Install [Docker](https://hub.docker.com/editions/community/docker-ce-desktop-mac)
6. Run Docker. In Docker's Preferences, under the Advanced tab, you may need to up the resources.
7. **git clone --recurse-submodules https://github.com/crr0004/deepracer.git**

## Minio Setup

8. In Finder, navigate to your cloned repo. It should be at ~/deepracer
- Navigate to folder rl_coach
- Using any code or text editor, open env.sh
- On lines 3 and 4, replace minio and miniokey with your AWS access key and password
- On line 12, replace "$(hostname -i)" with your actual IP address (ie: 192.168.1.xxx)
- On line 14, replace "readlink" with "greadlink" and save the file
9. **cd deepracer/rl_coach**
10. **brew install coreutils**
11. **. ./env.sh**
12. **minio server data**
13. In a web browser, navigate to your Minio server (http://127.0.0.1:9000) and login using "minio" and "miniokey"
14. Create a bucket called "bucket"
15. In Finder, navigate to ~/deepracer/rl_coach/data. There should now be a bucket folder. Within the bucket folder, create a new folder called "custom_files". Place your reward.py file in /custom_files. This should automatically upload to your Minio bucket.

## Sagemaker Setup

16. From Terminal, open a new shell (command+T)
17. **cd ..** to return to ~/deepracer
18. **python3 -m venv sagemaker_venv**
19. **. sagemaker_venv/bin/activate**
20. **pip install PyYAML==3.11**
21. **pip install urllib3==1.21.1**
22. **pip install -U sagemaker-python-sdk/ awscli ipython pandas**
23. **docker pull crr0004/sagemaker-rl-tensorflow:console**
24. **docker tag crr0004/sagemaker-rl-tensorflow:console 520713654638.dkr.ecr.us-east-1.amazonaws.com/sagemaker-rl-tensorflow:coach0.11-cpu-py3**
25. **mkdir -p ~/.sagemaker && cp config.yaml ~/.sagemaker**
26. **cd rl_coach**
27. **export LOCAL_ENV_VAR_JSON_PATH=$(greadlink -f ./env_vars.json)**
28. **mkdir ~/robo**
29. **mkdir ~/robo/container**
30. In ~/deepracer/rl_coach, open rl_deepracer_coach_robomaker.py in an editor. 
- Make sure that your endpoint_url (line 27) is the url to your minio server (ie: 192.168.1.xxx:9000)
- For CPU training, make sure instance_type (line 92) is "local" and image_name (line 108) is "crr0004/sagemaker-rl-tensorflow:console"
- Save the file
30. **ipython rl_deepracer_coach_robomaker.py**

## Robomaker Setup

31. From Terminal, open a new shell (command+T)
32. **cd ..** to return to ~/deepracer
33. **. sagemaker_venv/bin/activate**
34. **cd rl_coach**
35. **. ./env.sh**
36. **docker pull crr0004/deepracer_robomaker:console**
37. **cd ..**
38. In ~/deepracer, open robomaker.env in an editor.
- Again, make sure that your S3_ENDPOINT_URL is the url to your minio server (ie: 192.168.1.xxx:9000)
- Save the file
39. **docker run --rm --name dr --env-file ./robomaker.env --network sagemaker-local -p 8080:5900 -it crr0004/deepracer_robomaker:console**

## Watch the training

40. Open VNC Viewer
41. Connect to 127.0.0.1:8080
42. Hope everything worked out and training starts after everything is loaded.

# NOTES
- You should have three Terminal windows open during training
  1. Minio Server
  2. Training Episodes/Policy Training logs
  3. Step logs
- The terminal with the step logs will repeat a message until the policy training is complete.
- These steps are for CPU training. CPU training will slow down during the policy training. Each policy training session after a training iteration will get progressively slower. 
- After setting up, you can follow [these steps](https://github.com/kevinmarlis/deep-racer/blob/master/Mac-local-training.md) to train a new model.
- Please let me know if something needs revising, I've missed a step, or something isn't clear!
