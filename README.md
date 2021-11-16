# CMPE 172 - Lab #10 Notes

### CI Workflow
* Create CI workflow

![CI](images/create_ci_workflow.png)	
<p>&nbsp;</p>

* Test CI

![CI](images/test_ci.png)
<p>&nbsp;</p>

### CD Workflow
* Create service account

 ![CI](images/create_service_account.png)
<p>&nbsp;</p>

* Create Key

![CI](images/key.png)
<p>&nbsp;</p>

* GitHub Action Secrets

![CI](images/gke_serect.png)
<p>&nbsp;</p>

* GitHub Action google.yml

![CI](images/google_yml.png)
<p>&nbsp;</p>

* Make sure cluster is up

![CI](images/gke_cluster.png)
<p>&nbsp;</p>

* Trigger a CD Deployment by creating a new GitHub Release

![CI](images/gke_cluster.png)
<p>&nbsp;</p>

* Releases
![CI](images/release_2.4.png)
<p>&nbsp;</p>


* Deploy failed
  ![CI](images/deploy_fail.png)
<p>&nbsp;</p>

* Release 2.5
![CI](images/release_2.5.png)
<p>&nbsp;</p>

* Publish 2.5
![CI](images/publish_2.5.png)
<p>&nbsp;</p>

* Publish 2.5 failed
  ![CI](images/fail_2.5.png)
<p>&nbsp;</p>


* Publish succeed
  ![CI](images/setup_deploy_succeed.png)
<p>&nbsp;</p>

![CI](images/all_workflow.png)
<p>&nbsp;</p>

* Workloads
  ![CI](images/workloads.png)
<p>&nbsp;</p>

* Services
  ![CI](images/services.png)
<p>&nbsp;</p>

* Create Ingress
 ![CI](images/create_ingress.png)
<p>&nbsp;</p>

![CI](images/ingress_up.png)
<p>&nbsp;</p>

* Run the app

![CI](images/app.png)
<p>&nbsp;</p>



------------------------------------------------------------
###### Try to create service account again with Owner role only

* Service account details

![CI](images/services_account_owner.png)
<p>&nbsp;</p>

* Updated key
![CI](images/updated_key.png)
<p>&nbsp;</p>

* Create_release_2.7
![CI](images/create_release_2.7.png)
<p>&nbsp;</p>

* Published
  ![CI](images/published_2.7.png)
<p>&nbsp;</p>

  ![CI](images/build_2.7.png)
<p>&nbsp;</p>

![CI](images/building.png)
<p>&nbsp;</p>

![CI](images/building_1.png)
<p>&nbsp;</p>

![CI](images/building_1.png)
<p>&nbsp;</p>

* Release 2.7 failed
* Deleted current service account
* Enable APIs: https://console.cloud.google.com/apis/enableflow?apiid=containerregistry.googleapis.com,container.googleapis.com&project=cmpe172-326704
* Enables API : https://console.cloud.google.com/apis/enableflow?apiid=iam.googleapis.com&redirect=https:%2F%2Fconsole.cloud.google.com&_ga=2.60381738.647820183.1636998019-1134740736.1635659677&project=cmpe172-326704
* Create new service account
  * Name = spring-gumball-l1
    * Roles:
      * Owner
      * Kubernetes Engine Developer
      * Storage Admin
      * Service Account Admin
    * Create key
  * Wait 2- 5 minutes
* Update GKE_SA_KEY
* Releases 2.8

![CI](images/releases_2.8.png)
<p>&nbsp;</p>

![CI](images/releases_2.8_1.png)
<p>&nbsp;</p>

* The results in GKE are the same as the above


## Lab Notes

### CI Workflow

* Reference: https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-java-with-gradle
* Continuous integration (CI) 
* Objectives:
	*  Create a workflow that performs continuous integration for Java project using Gradle build system.
	* The workflow you create will allow you:
		* To see when commits to a pull request cause build or test failures against your default branch.
		* Ensure that your code is always healthy.
		* Can extend CI workflow to caches files and upload artifacts from a workflow run.
* Create a CI workflow to trigger on push and pr on main
    * Select Action -> Find Java with Gradle -> Set up this workflow
    * Make change to the predefined yml file
    * Make sure the spring-gumball is at root level; otherwise will run into the issue
      * with Grant execute permission for gradlew
      
### CD Workflow
* Make sure to have these files in the repo:
  * deployment.yaml
  * services.yaml
  * kustomization.yml
* Login to Google Cloud
  * Create  a cluster
  
* Set up secrets in your workspace
    * Go to IAM&Admin -> Service Accounts -> Create service account
    * Fill out the service account name with: spring-gumnall
    * Grant this service account access to project
      * Type: Service Account Admin
      * Done
    * Save the key
    * Go to Github
      * Setting -> Secret -> new repository secret 
      * GKE_PROJECT; value = cmpe172-326704 ; GKE_PROJECT: Google Cloud project ID can be found in the Json file of the Key
      * GKE_SA_KEY; value = KEY from service account
      
![CI](images/project_id.png)
<p>&nbsp;</p>


* Create google.yml     
  * GitHub -> Action -> New workflow ->Build and Deploy to GKE -> Set up this workflow
  * edit the template
  * Start commit -> Commit new file

###### Trigger a CD Deployment by creating a new GitHub Release
* GItHub -> root folder -> Release -> create a new release
* Choose a tag: 2.0 create a new tag
* Release title : 2.0 -> Publish release

* Run google-github-actions/get-gke-credential@v0.2.1 error

![CI](images/google_yml_error.png)
<p>&nbsp;</p>

*  Solution: delete the service account and create an new one with Owner permission

![CI](images/owner_permission.png)
<p>&nbsp;</p>


* Publish error ; Caller does not have permission 'storage.buckets.get
![CI](images/publish_error.png)
<p>&nbsp;</p>

* Process to fix the error:
  * Added different roles
  * Deleted the service account
  * Created  new ones
  * Using the default service account
  
Solution:
* Using the default service account
* correct GKE_PROJECT = cmpe172-326704


##### What are service accounts?
* A service account is a special kind of account used by an application or compute workload
* Applications use services accounts to make authorized API calls, authorized as either the services account itself
    * or as Google Workspace or Cloud Identity users through domain-wide delegatio.
* a Compute Engine VM can run as a service account,and that account can be given permissions to access the resources it needs.
* The service account is the identity of the service
* The service account's permissions control which resources the service can access
* A service account is identified by its email address, which is unique to the account

* Select Action -> find Build and Deploy to GKE -> Set up this workflow
	

