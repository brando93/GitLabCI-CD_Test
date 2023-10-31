# GitLabCI-CD_Test

.gitlab-ci.yml

stages: 
    - .pre
    - build
    - test

build website:
    image: node:16-alpine
    stage: build
    artifacts:
        paths:
            - build
    script:
        - yarn install
        - yarn build

.linter:
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

.unit tests:
    image: node:16-alpine
    stage: .pre
    script:
        - yarn install        
        - yarn test
