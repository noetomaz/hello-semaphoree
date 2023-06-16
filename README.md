# hello-semaphore
For trial
version: v1.0
name: Initial Pipeline
agent:
  machine:
    type: e1-standard-4
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
        - name: 'Job #1'
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
