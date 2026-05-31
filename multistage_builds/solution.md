1.  Update the content of the file index.html under the same repository to Welcome to xFusionCorp Industries and push the changes to the origin into the master branch.
2.  Apache is already installed on the app server and is running on port 8080.
3.  Add App Server 1 as a Jenkins agent (slave) node: name App Server 1, label stapp01, remote root directory /home/sarah/jenkins_agent, launch via SSH with host stapp01 and credentials for user sarah. Install java-17-openjdk on App Server 1 if needed.
4.  Create a Jenkins pipeline job named deploy-job (it must not be a Multibranch pipeline job) and pipeline should have two stages Deploy and Test ( names are case sensitive ). Configure these stages as per details mentioned below.
    +  The Deploy stage should deploy the code from web repository under /var/www/html on App Server 1, as this is the document root of the app server.
    +  The pipeline should run on the App Server 1 node (e.g. use label stapp01).
    +  The Test stage should just test if the app is working fine and website is accessible. Its up to you how you design this stage to test it out, you can simply add a curl command as well to run a curl against the LBR URL (http://stlb01:8091) to see if the website is working or not. Make sure this stage fails in case the website/app is not working or if the Deploy stage fails.
  


**Solution**
1.  Go to your Jenkins Dashboard and navigate to Manage Jenkins -> Nodes -> New Node.
2.  Launch agents via SSH  (If not there add with plugins)
3.  Go to app server and install java - ```sudo yum install -y java-17-openjdk```

**Pipeline.sh**
-------------
```
    pipeline {
    agent {
        label 'stapp01' // Restricts execution specifically to App Server 1
    }

    stages {
        stage('Deploy') {
            steps {
                echo 'Starting deployment...'
                dir('/var/www/html') {
                    // Changed branch from 'main' to 'master' based on standard lab defaults
                    git branch: 'master', url: 'https://3000-port-7pxwlgtv7y2ljk5c.labs.kodekloud.com/sarah/web.git'
                }
                echo 'Deployment completed successfully.'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing application accessibility via Load Balancer...'
                script {
                    // Test if the website is accessible via the Load Balancer
                    sh 'curl -f --retry 3 http://stlb01:8091'
                }
            }
        }
    }
    
    post {
        failure {
            echo 'Pipeline failed. Please check the logs above for deployment or testing errors.'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
    }
}
    ```
