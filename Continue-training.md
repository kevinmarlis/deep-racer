
## Continue training a model

1. In deepracer/rl_coach/data/bucket, create new folder called rl-deepracer-pretrained. Within rl-deepracer-pretrained, create new folder called model.

2. From deepracer/rl_coach/data/bucket/rl-deepracer-sagemaker/model, copy the following five files into rl-deepracer-pretrained/model (where X_Step is the most recently completed):

  - X_Step-Y.ckpt.index
  - X_Step-Y.ckpt.meta
  - X_Step-Y.ckpt.data-00000-of-00001
  - model_metadata.json
  - checkpoint

3. In deepracer/rl_coach, edit rl_deepracer_coach_robomaker.py. Uncomment the two pretrained lines and save the file:
  - "pretrained_s3_bucket": "{}".format(s3_bucket),
  - "pretrained_s3_prefix": "rl-deepracer-pretrained"

4. Train as normal.
