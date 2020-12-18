# move-vars-azure-pipeline
A small cheatsheet on how to move variables and parameters across Azure Pipelines

``` YAML
name: "test"
parameters:
  - name: parameter1
    type: string
    default: windows
variables:
  var1: valueVAR1
  var2: ${{ parameters.parameter1 }}
stages:
    - stage: justAStep
      displayName: I am the fist step
      jobs:
        - job: Testjob
          strategy:
           matrix:
             windows: 
               var2 : 'windows'
             ubuntu:
               var2: 'ubuntu'

          pool:
            vmImage: ${{ variables.var2 }}-latest
          steps:
          - script: echo the 1st variaeble is $(var1)
          - script: echo the 1st parameter is $(parameter1)
          - bash: echo $(var1)
          - task: PowerShell@2
            inputs:
              targetType: 'inline'
              script: |
               Write-Host this is the 2nd variable $(var1)
```
