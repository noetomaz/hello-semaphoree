# hello-semaphore
For trial
version: v2.0
name: Initial Pipeline
agent:
  machine:
    type: e1-standard-2
    os_image: ubuntu2004
blocks:
  - name: 'Pretest #2'
    task:
      jobs:
        - name: 'Job #1'
          commands: []
          parallelism: 3
        - name: 'Job #2'
          commands: []
        - name: 'Job #3'
          commands: []
        - name: 'Job #4'
          commands:
            - checkout
    dependencies: []
  - name: 'Pretest #1'
    dependencies: []
    task:
      jobs:
        - name: 'Job #1 - mcap'
          commands: []
  - name: Unit Test
    dependencies:
      - 'Pretest #1'
      - 'Pretest #2'
    task:
      jobs:
        - name: 'Job #1'
          commands: []
  - name: Integration Test
    dependencies:
      - Unit Test
    task:
      jobs:
        - name: 'Job #1'
          commands: []
        - name: 'Job #2'
          commands: []
  - name: 'Block #5'
    dependencies:
      - 'Pretest #2'
      - Unit Test
    task:
      jobs:
        - name: 'Job #1'
          commands: []
after_pipeline:
  task:
    jobs:
      - name: 'Job #1'
        commands: []
promotions:
  - name: Promotion 1
    pipeline_file: pipeline_2.yml
    auto_promote:
      when: branch = 'master' AND result = 'passed'
    apiVersion: v1alpha
kind: Project
metadata:
  name: goDemo
spec:
  repository:
    url: "git@github.com:renderedtext/goDemo.git"
    run_on:
      - branches
      - tags
    pipeline_file: ".semaphore/semaphore.yml"
    status:
      pipeline_files:
        - path: ".semaphore/semaphore.yml"
          level: "pipeline"
  tasks:
    - name: first-scheduler
      scheduled: true
      branch: master
      at: "5 4 * * *"
      pipeline_file: ".semaphore/cron.yml"
      status: ACTIVE
    - name: second-task
      description: "second-task description"
      scheduled: false
      branch: develop
      at: ""
      pipeline_file: ".semaphore/canary.yml"
      status: ACTIVE
      parameters:
      - name: CANARY_VERSION
        required: true
        default_value: "1.0.0"
        
