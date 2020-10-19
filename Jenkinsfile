
pipeline {
    agent any
   
    tools {nodejs "nodejs"}

    stages {
        
         stage ('Artifactory configuration') {
            steps {
                rtServer (
                    def server = Artifactory.server 'central1'
                )
                rtNpmResolver (
                    id: "NPM_RESOLVER",
                    serverId: "central1",
                    repo: "aero-repo"
                )

                rtNpmDeployer (
                    id: "NPM_DEPLOYER",
                    serverId: "central1",
                    repo: "aero-repo"
                )
            
            }

        }

         stage('install') { 
            steps {
                sh 'npm install' 
            }
        }
        stage('test'){
            steps{
                sh 'npm run test'
            }
        }
        stage ('build'){
            steps{
                sh 'npm run build'
            }
        }
        stage('package'){
            steps{
                sh 'rm -rf reactapp'
                sh 'mkdir reactapp'
                sh 'cp -R build /var/jenkins_home/workspace/react_app_build_clone_main/reactapp'
                sh 'cp package.json /var/jenkins_home/workspace/react_app_build_clone_main/reactapp'
                
            }
        }
        stage ('Exec npm publish') {
            steps {
                rtNpmPublish (
                    tool: "nodejs", // Tool name from Jenkins configuration
                    path: "/var/jenkins_home/workspace/react_app_build_clone_main/reactapp",
                    deployerId: "NPM_DEPLOYER"
                )
            }
        }
    }
}