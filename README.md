# GitLabCI-CD_Test
GitLabCI-CD_Test

```
.gitlab-ci.yml

stages: 
    - .pre
    - build
    - test
    - deploy

build website:
    image: node:16-alpine
    stage: build
    artifacts:
        paths:
            - build
    script:
        - yarn install
        - yarn build

linter:
    image: node:16-alpine
    stage: .pre
    script:
        - yarn install
        - yarn lint

test website:
    image: node:16-alpine
    stage: test
    script:
        - apk add curl
        - yarn global add serve
        - serve -s build & 
        - sleep 10
        - curl http://localhost:3000 | grep -i "title"

deploy to s3:
    stage: deploy
    image:
        name: amazon/aws-cli:2.4.11
        entrypoint: [""]
    script:
        - aws --version
        - aws s3 sync build s3://$AWS_S3_BUCKET --delete


unit tests:
    image: node:16-alpine
    stage: .pre
    script:
        - yarn install        
        - yarn test

```
