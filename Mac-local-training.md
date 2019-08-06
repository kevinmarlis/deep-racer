After setting up from [here](https://github.com/kevinmarlis/deep-racer/blob/master/Mac-Local-Training-Installation.md) 

#### Bold lines are terminal commands

Make sure Docker is running.

## Minio

1. In a terminal window, redirect to ~/ if need be
2. **cd deepracer**
3. **cd rl_coach**
4. **. ./env.sh**
5. **minio server data**

## Sagemaker

6. From Terminal, open a new shell (command+T)
7. **cd ..** to return to ~/deepracer
8. **python3 -m venv sagemaker_venv**
9. **. sagemaker_venv/bin/activate**
10. **pip install -U sagemaker-python-sdk/ awscli ipython pandas**
11. **docker pull crr0004/sagemaker-rl-tensorflow:console**
12. **docker tag crr0004/sagemaker-rl-tensorflow:console 520713654638.dkr.ecr.us-east-1.amazonaws.com/sagemaker-rl-tensorflow:coach0.11-cpu-py3**
13. **mkdir -p ~/.sagemaker && cp config.yaml ~/.sagemaker**
14. **cd rl_coach**
15. **export LOCAL_ENV_VAR_JSON_PATH=$(greadlink -f ./env_vars.json)**
16. **ipython rl_deepracer_coach_robomaker.py**

## Robomaker 

17. From Terminal, open a new shell (command+T)
18. **cd ..** to return to ~/deepracer
19. **. sagemaker_venv/bin/activate**
20. **cd rl_coach**
21. **. ./env.sh**
22. **docker pull crr0004/deepracer_robomaker:console**
23. **cd ..**
24. **docker run --rm --name dr --env-file ./robomaker.env --network sagemaker-local -p 8080:5900 -it crr0004/deepracer_robomaker:console**

## Watch the training

25. Open VNC Viewer
26. Connect to 127.0.0.1:8080
