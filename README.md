# CICD implementation for ADF Notes (In progress)

### Summary of steps in configuring  CI-CD for ADF
1. Choose  Default directory
2. Create Organization
3. Create  Project  - under this we have all the  repos and deployment pipeline will  be present
4. Create and intialize repo
5. Connect ADF canvas with Repo.
        - Once it is connected, you can see the Azure Live environment
6. Now create a feature branch ,add the changes to  it
7. Once development is done, rasie PR to merge the feature with main branch(collaboration)
8. Click Publish to manually deploy the artifacts to LIVE ENVIRONMENT
9. Create Folder & add the NPM package to support Automated publish
10. Create Pipeline & update NPM package location, subscription details
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
          - INcremental option
          - Provide the template location and parameter
