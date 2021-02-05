pipeline
{
    agent any
    stages
    {
        stage('ContDownload') 
        {
            steps
            {
                script
                {
                    try
                    {
                        git 'https://github.com/shivaramyadav/maven.git'
                    }
                    catch(Exception e)
                    {
                        mail bcc: '', body: 'jenkins could not download the code from git repository', cc: '', from: '', replyTo: '', subject: 'Git Download failed', to: 'gitadmin@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContBuild')
        {
            steps
            {
                script
                {
                    try
                    {
                        sh 'mvn package'
                    }
                    catch(Exception e)
                    {
                        mail bcc: '', body: 'jenkins unable create an artifact', cc: '', from: '', replyTo: '', subject: 'Git Build failed', to: 'developer@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContDeployment')
        {
            steps
            {
                script
                {
                    try
                    {
                        sh 'scp /root/.jenkins/workspace/DeclarativePipeline/webapp/target/webapp.war ubuntu@172.31.60.147:/var/lib/tomcat8/webapps/testenv.war'
                    }
                    catch(Exception e)
                    {
                        mail bcc: '', body: 'jenkins unable to deploy into qaserver', cc: '', from: '', replyTo: '', subject: 'Git Deployment failed', to: 'middleware@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContTesting')
        {
            steps
            {
                script
                {
                    try
                    {
                        git 'https://github.com/shivaramyadav/testing.git'
                        sh 'java -jar /root/.jenkins/workspace/DPipeline/testing.jar'
                    }
                    catch(Exception e)
                    {
                        mail bcc: '', body: 'testing of the jenkins has failed ', cc: 'developers@gmail.com', from: '', replyTo: '', subject: 'Testing failed', to: 'testers@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContDelivery')
        {
            steps
            {
                script
                {
                    try
                    {
                        sh 'scp /root/.jenkins/workspace/DeclarativePipeline/webapp/target/webapp.war ubuntu@172.31.63.233:/var/lib/tomcat8/webapps/delenv.war'
                    }
                    catch(Exception e)
                    {
                        mail bcc: '', body: 'jenkins could not deploy to the Prod Environment for delivery', cc: '', from: '', replyTo: '', subject: 'Delivery failed', to: 'deliveryManager@gmail.com'
                        exit(1)
                    }
                }
            }
        }
    }
}
