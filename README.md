# CICD implementation for ADF Notes (In progress)

### Summary of steps in configuring  CI-CD for ADF
[Reference](https://bulletbyte.weebly.com/tech/how-to-set-up-cicd-for-azure-data-factory-using-azure-devops)
1. Choose  Default directory
2. Create Organization
3. Create  Project  - under this we have all the  repos and deployment pipeline will  be present
4. Create and intialize repo
5. Connect ADF canvas with Repo.
        - Once it is connected, you can see the Azure Live environment
6. Now create a feature branch ,add the changes to  it
7. Once development is done, rasie PR to merge the feature with main branch(collaboration)
8. Click Publish to manually deploy the artifacts to LIVE ENVIRONMENT
9. Create Folder - add the NPM package to support Automated publish
10. Create Pipeline - to update NPM package location, subscription details
      Note: enable parallelism before triggering the pipeline
11. Running the pipeline will publish the ADF pipeline, thereby ARM template will be created.It will not PUBLISH ARM template
     to LIVE environment.
12. Use CLASSIC RELEASE pipeline to deploy,the changes to LIVE environment.Note: this can be done using a YAML pipeline (it is recommended approach)
        - Enable the classic RELEASE PIpeline at the organisational level
13. Create EMPTY JOB under the RELEASE pipeline
14. Mention the SOURCE type as BUILD, as we need to take the latest build  from PIPELINE.
- Choose LATEST version as we need the latest version from the BUILD
15. Choose the ARM DEPLOYMENT in AGENT JOB
- To connect the Azure DEVOPs with Resource Group , create a  SERVICE PRINCIPAL with Contributor Role to the Resource Group
- Incremental option
- Provide the template location and parameter.
### Notes or things learned
1. Azure Devops pipeline can be a YAML pipeline or Classic build and release pipeline.Best Practise to use YAML pipeline due YAML code can be review, support runtime parameters. By default, the CLASSIC RELEASE pipelines are disables it has to be enabled.[MORE ON](https://devblogs.microsoft.com/devops/disable-creation-of-classic-pipelines/)
2. By Default, for private project there is only one pipeline allowed. You can request for more number of parallel pipeline. 
[MORE ON](https://learn.microsoft.com/en-us/azure/devops/pipelines/licensing/concurrent-jobs?view=azure-devops&tabs=ms-hosted#how-much-do-parallel-jobs-cost)

3. 2 types of deployment option available for Data Factory Artifacts.
- Manual deployment,this is done by  clicking on the PUBLISH button and ARM generated,will be manually exported and uploaded  to other ADF environment. Not a recommended approach, as HIGHER environment ADF will not allow to manually upload ARM.[Reference](https://www.youtube.com/watch?v=oHIBFMchMpw)
[CI with Manual deployment](https://www.youtube.com/watch?v=oL9l_BYMhR0&t=1790s)
- Automated deployment, raise a PR to merge the feature branch with main branch. Once it is code is merge that would trigger a build and release pipeline. Release pipeline is  responsible for deployment of ADF artifacts to higher environment. There are multiple patterns here. 
        [Without using NPM package](https://www.youtube.com/watch?v=jJcikWOUqOk)
        [With NPM package,YAML pipeline](https://manjitsingh664.medium.com/automated-ci-cd-for-azure-data-factory-7d80e1cb84ef)

4. Common branching statergy pattern is as below.

 ```feature branch(development branch)   ----> PR ---->  main branch(collaboration branch)----> Publish button ----> adf_publish(generated  ARM template)```

5. Follow a standard naming convetion for all the ADF artifacts, so it easy  to deploy to higher environment.
6. ARM deployment does not support Selective deployment. [Solution for this](https://azureplayer.net/2019/06/deployment-of-azure-data-factory-with-azure-devops/)
7. ARM template deployment Drawbacks:
 - if you delete a pipeline , it will not be reflected in the ADF Live environment.
 - if you have a active trigger, it  will be in Stopped state.
8. Solution to above problem is to handle this in POST and PRE deployment script.
### Demo on ADF CI CD ,Automated Publishing using NPM package,Classic build and Release pipeline
#### What are we trying to achieve?
![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)
