
## Integrate Playwright with Azure Devops Pipeline
There are 2 options, option1 is using yaml file & option2 is without using yaml file. let's see one by one

1. Option1 - Using YAML File
   - Step1: Create a new project in ADO then Click on Project
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/0ec3b6b7-748f-4d0a-80bf-762e24728afb)

   - Step2: Click on Repos & Let's create new repository, Click on New reposiotry
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/fe0485c8-2708-456b-9030-a046b1170c70)

   - Step3: Enter Repository name & Click on Create
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/ea15010a-4308-41c2-883e-f0ddee48908f)
    ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/0ee53f40-2d9e-4dbb-8301-5cc2c615d647)

   - Step4: Click on Clone button and get the URL. Go to your system then clone repository.
   - Step5: Add all the playwright framework folders inside cloned repository
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/b8039254-cba5-46ff-9696-0aad20dd9876)

   - Step6: Push all the folders into Azure devops
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/add3e34a-5ba8-4792-9d2c-dbae06bc6a64)

   - Step7: Repository is ready now, let's create pipeline. Click on Pipelines->Create Pipeline
   - ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/7bb2f8dc-8253-46ab-879a-743446211bdf)

   - Step8: Click on Azure Repos Git
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/885628e1-8e4c-43fc-ba6a-6125ec34e6fb)

   - Step9: Select previously created repository
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/09b1489d-f699-4885-84a4-c06554adc3e6)

   - Step10: Select Starter Pipeline
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/3db45ed6-c0c9-4033-b786-b8ca7e263ce4)

   - Step11: Copy below yaml content and paste it inside azure-pipelines.yml file. 
```
trigger:
- main

pool:
  vmImage: ubuntu-latest

steps:
- task: NodeTool@0
  inputs:
    versionSpec: '18'
  displayName: 'Install Node.js'
- script: npm ci
  displayName: 'npm ci'
- script: npx playwright install --with-deps
  displayName: 'Install Playwright browsers'
- script: npx playwright test
  displayName: 'Run Playwright tests'
  env:
    CI: 'true'
```
If you are running in self hosted agent replace pool commands
```
pool:
   name: AgentPoolName
   demands:
   - agent.name -equals AgentName
```
   - Step12: Click on Save and run
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/208f9b43-735a-45e1-b5c3-699df9e6d8f2)
    ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/41262f5d-6e80-4274-a4fc-75d0536e73a7)

   - Step13: You will see job queued like this.
   - ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/1fff0860-2ac5-45b0-aa45-757afb76777e)

   - Step14: Click on Job & Verify build status.
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/66326c8f-d789-4856-b90c-8909bef95930)

   - Step15: Now let's Upload playwright-report folder with Azure Pipelines & Report generation
     Firstly update azure-pipelines.yml file
```
trigger:
- main

pool:
  vmImage: ubuntu-latest

steps:
- task: NodeTool@0
  inputs:
    versionSpec: '18'
  displayName: 'Install Node.js'
- script: npm ci
  displayName: 'npm ci'
- script: npx playwright install --with-deps
  displayName: 'Install Playwright browsers'
- script: npx playwright test
  displayName: 'Run Playwright tests'
  env:
    CI: 'true'

- task: PublishTestResults@2
  displayName: 'Publish test results'
  inputs:
    searchFolder: 'test-results'
    testResultsFormat: 'JUnit'
    testResultsFiles: 'e2e-junit-results.xml'
    mergeTestResults: true
    failTaskOnFailedTests: true
    testRunTitle: 'My End-To-End Tests'
  condition: succeededOrFailed()
- task: PublishPipelineArtifact@1
  inputs:
    targetPath: playwright-report
    artifact: playwright-report
    publishLocation: 'pipeline'
  condition: succeededOrFailed()
```
     
   - Step16: Verify playwright-report folder attachment & report.
       From job we can navigate into artifacts folder. Download playwright report and verify results.
    ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/54aaf4b4-7715-435d-b96a-7a19c23fa384)
    ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/5ed6c543-4162-43a9-9f21-9643d7f52438)

   
2. Option2 - Without using YAML File
   - Step1: Repeat step 1 to 6 above
   - Step2: Click on Pipelines then click on New Pipeline
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/1f753af4-881e-495d-a7dd-8c9163de97ff)

   - Step3: Click on Use the classic editor & Click on Continue
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/499f6cf4-0458-4aba-813a-ad131cea4b02)
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/332011f7-0ae4-46ce-a9c2-3c2a9d66f599)

   - Step4: Click on Emtpy job
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/d84dda85-cbc0-4c9f-9147-bc1de36823c4)

   - Step5: Click on + icon, Search for Node and add Node.js tool installer
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/73d32c4d-2cd1-4f78-beb7-ec1bf5f5138a)
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/64fcf35f-1200-4ccf-b34c-d53072728ced)


   - Step6: Select just now added task and add Node v16 becuase playwright supports for Node v14 and above
   - ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/aa804427-464c-434f-b4e4-27547b245bd9)

   - Step7: Click on + icon, Similary add Command line task,
     Display name: Install Playwright & Dependencies
     Script: npm install && npx playwright install
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/df63a628-ccb4-4709-8c2a-166358dc5264)
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/70991c9e-ad21-4ab8-978e-ba02d0f5971f)

     Click on Advanced-> Click on little icon(i) & select the Link. This will enable working directory for the task.
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/1d6dc42d-e720-4446-b1f7-4589f105ff04)

   - Step8: Add another task by clicking on + icon, search for npm & Add npm
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/49eadf73-d640-4c7d-8ea6-730f2291d503)

     Enter Display name, Select Command as custom & Enter Command and Arguments as run tests
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/f055ace0-8cdb-46e7-9f24-33c808eef4ee)

     In this task we are referring to the package.json file.
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/6074a566-6efb-46a7-ad37-2108eed90bf8)

   - Step9: Once everthing is completed now it is a time run script. Click on Save & queue.
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/112334f9-6adb-43a5-b2d4-41f364c7527d)

    Add commit message then click save & run.
   
   - Step10: It looks like this
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/ae48637e-d0bf-4a32-8d93-2ea251301068)

     Click on Job and you will see a screen like this
     ![image](https://github.com/BakkappaN/PlaywrightTutorialFullCourse/assets/22426896/b135a6c0-039c-4b90-934c-849b35e47cbc)




  
  
