# Jenkinsfile

The Jenkinsfile to run E2E tests from [cypress-demo](https://github.com/HuitingHuang/cypress-demo.git)

The Jenkins pipeline can be parameterized to decide:
<br>- on which browser to run tests
<br>- which spec files to run filtered by file path
<br>- which test suites to run filtered by tags
<br>- how many parallel jobs running at the mean time

This job is parameterized like the following:
<br>
<img src="screenshots/job-parameters.png" width="50%" />
<br>
<br>
The job build view is like the following:
<br>
<img src="screenshots/Jenkins-build-view2.png" width="50%" />
<br>
<br>
The Cypress dashboard received the test results from the Jenkins pipeline and shows like the following:
<br>
<img src="screenshots/cypress-dashboard2.png" width="50%" />


