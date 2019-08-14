After setting up from [here](https://github.com/kevinmarlis/deep-racer/blob/master/Mac-Local-Training-Installation.md) 

#### Bold lines are terminal commands

Make sure Docker is running.

## Minio

1. In Terminal, navigate to deepracer/rl_coach (**cd "your path to deepracer/rl_coach"**)
2. **. ./env.sh**
3. **minio server data**

## Sagemaker

4. From Terminal, open a new shell (command+T)
5. **cd ..** to return to ~/deepracer
6. **. sagemaker_venv/bin/activate**
7. **docker pull crr0004/sagemaker-rl-tensorflow:console**
8. **docker tag crr0004/sagemaker-rl-tensorflow:console 520713654638.dkr.ecr.us-east-1.amazonaws.com/sagemaker-rl-tensorflow:coach0.11-cpu-py3**
9. **mkdir -p ~/.sagemaker && cp config.yaml ~/.sagemaker**
10. **cd rl_coach**
11. **export LOCAL_ENV_VAR_JSON_PATH=$(greadlink -f ./env_vars.json)**
12. **ipython rl_deepracer_coach_robomaker.py**

## Robomaker 

13. From Terminal, open a new shell (command+T)
14. **cd ..** to return to ~/deepracer
15. **. sagemaker_venv/bin/activate**
16. **cd rl_coach**
17. **. ./env.sh**
18. **docker pull crr0004/deepracer_robomaker:console**
19. **cd ..**
20. **docker run --rm --name dr --env-file ./robomaker.env --network sagemaker-local -p 8080:5900 -it crr0004/deepracer_robomaker:console**

## Watch the training

21. Open VNC Viewer
22. Connect to 127.0.0.1:8080
