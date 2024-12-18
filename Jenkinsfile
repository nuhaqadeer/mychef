pipeline {
    agent any
    environment {
        // Use the stored GitHub token from Jenkins Credentials
        GITHUB_TOKEN = credentials('github-pat')  // 'github-pat' is the ID of the stored credential
        REPO_URL = 'https://github.com/nuhaqadeer/chef-repo.git'  // Replace with your GitHub repo URL
        CHEF_HOME = '/path/to/chef'  // Replace with your Chef setup path if needed
    }
    stages {
        stage('Clone Repository') {
            steps {
                script {
                    // Clone the repository using the GitHub token for authentication
                    withCredentials([usernamePassword(credentialsId: 'github-pat', usernameVariable: 'USERNAME', passwordVariable: 'GITHUB_TOKEN')]) {
                        sh 'git clone https://$USERNAME:$GITHUB_TOKEN@github.com/nuhaqadeer/chef-repo.git'
                    }
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install Chef or any dependencies for your environment
                    sh 'sudo apt-get update && sudo apt-get install -y chef'
                }
            }
        }

        stage('Run Chef Client') {
            steps {
                script {
                    // Runs Chef client with the specified recipe
                    sh 'chef-client --chef-license accept --runlist "recipe[my_cookbook::default]"'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deployment step to apply Chef configuration
                    sh 'chef-client --chef-license accept --runlist "recipe[my_cookbook::deploy]"'
                }
            }
        }
    }
    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
