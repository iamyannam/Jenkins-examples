The DevOps team was looking for a solution where they want to restart Apache service on all app servers if the deployment goes fine on these servers in Stratos Datacenter. After having a discussion, they came up with a solution to use Jenkins chained builds so that they can use a downstream job for services which should only be triggered by the deployment job. So as per the requirements mentioned below configure the required Jenkins jobs.



Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and Adm!n321 password.


Similarly you can access Gitea UI on port 3000 (or click the Gitea button) and username and password for Git is sarah and Sarah_pass123 respectively. Under user sarah you will find a repository named web.


Apache is already installed and configured on the app server. The doc root /var/www/html on App Server 1 is a local git repository tracking the origin web repository.


1. Create a Jenkins job named devops-app-deployment and configure it to pull changes from the master branch of the web repository on App Server 1 under /var/www/html directory.


2. Create another Jenkins job named manage-services and make it a downstream job for devops-app-deployment. Things to take care about this job are:


a. This job should restart httpd service on the app server (App Server 1).

b. Trigger this job only if the upstream job i.e devops-app-deployment is stable.
