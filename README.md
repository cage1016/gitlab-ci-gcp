# gitlab-ci-gcp

> gitlab-ci-multi-runner base images for python

## Usage

__.gitlab-ci.yml__

```yml
image: cage1016/gitlab-ci-gcp:v1.4

before_script:
  - export CLOUDSDK_CORE_DISABLE_PROMPTS=1
  - export CLOUDSDK_PYTHON_SITEPACKAGES=1
  - export GCP_PROJECT=<your-gcp-project-id>

types:
  - test
  - deploy

test:
  stage: test
  script:
    - sh ./scripts/tests.sh

deploy:
  stage: deploy
  script:
    - sh ./scripts/deploy.sh
```

__./scripts/tests.sh__

```sh
#!/usr/bin/env bash

virtualenv env
source env/bin/activate

# install test env packages from requirements.testing.txt
pip install -r requirements.testing.txt

# link env as lib folder
pip install git+https://github.com/ze-phyr-us/linkenv.git
linkenv env/lib/python2.7/site-packages lib
```

__./scripts/deploy.sh__

```sh
#!/usr/bin/env bash

pip install -r requirements.txt -t lib/

echo $GCLOUD_KEY > key.json

gcloud auth activate-service-account $GCLOUD_ACCOUNT --key-file key.json
gcloud --quiet config set project $GCP_PROJECT
gcloud --quiet preview app deploy app.yaml --no-promote --version uat
```

## Author

© 2016 Kai-Chu Chung <cage.chung@gmail.com> (http://kaichu.io)

## License

Released under the [MIT license](http://cage1016.mit-license.org).
