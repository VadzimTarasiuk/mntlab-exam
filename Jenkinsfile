node("${env.SLAVE}") {

  stage("Build"){
    git branch: 'vtarasiuk', url: 'https://github.com/VadzimTarasiuk/mntlab-exam.git'
    sh  'echo "Build time = $(date)\nBuild Machine Name = $(hostname)\nBuild User Name = vtarasiuk\n" > ./src/main/resources/build-info.txt'
    sh 'echo "GIT URL: `git config --get remote.origin.url`\nGIT Commit: `git rev-parse HEAD`\nGIT Branch: `git rev-parse --abbrev-ref HEAD`" >> ./src/main/resources/build-info.txt '
    sh '/home/student/apache-maven-3.5.0/bin/mvn clean package -DbuildNumber=42'
  }

  stage("Package"){
    /*
        use tar tool to package built war file into *.tar.gz package
    */
    sh 'tar czf mnt-exam.tar.gz target/mnt-exam.war src/main/resources/build-info.txt'
    archiveArtifacts artifacts: '*.tar.gz', onlyIfSuccessful: true
  }

  stage("Roll out Dev VM"){
    /*
        use ansible to create VM (with developed vagrant module)
    */
    withEnv(["ANSIBLE_FORCE_COLOR=true", "PYTHONUNBUFFERED=1"]) {     
    ansiColor('xterm') {        
        
        sh "ansible-playbook createvm.yml -vv"    
      }
    }
  }

  stage("Provision VM"){
    /*
        use ansible to provision VM
        Tomcat and nginx should be installed
    */
    withEnv(["ANSIBLE_FORCE_COLOR=true", "PYTHONUNBUFFERED=1"]) {     
    ansiColor('xterm') {        
        
        sh "ansible-playbook provisionvm.yml -vv"    
      }
    }
  }

  stage("Deploy Artefact"){
    /*
        use ansible to deploy artefact on VM (Tomcat)
        During the deployment you should create file: /var/lib/tomcat/webapps/deploy-info.txt
        Put following details into this file:
        - Deployment time
        - Deploy User
        - Deployment Job
    */
    sh "echo 'Job name=${JOB_NAME}\n' > ./deploy-info.txt"
    sh 'echo "Deployment time= $(date)\nDeployment user = $(whoami)" >> ./deploy-info.txt'
    withEnv(["ANSIBLE_FORCE_COLOR=true", "PYTHONUNBUFFERED=1"]) {     
    ansiColor('xterm') {        
       
        sh "ansible-playbook deploy.yml -e artefact=./target/mnt-exam.war -vv"    
      }
    }
  }

  stage("Test Artefact is deployed successfully"){
    /*
        use ansible to artefact on VM (Tomcat)
        During the deployment you should create file: /var/lib/tomcat/webapps/deploy-info.txt
        Put following details into this file:
        - Deployment time
        - Deploy User
        - Deployment Job
    */
    sh 'echo $(vagrant status)'
    withEnv(["ANSIBLE_FORCE_COLOR=true", "PYTHONUNBUFFERED=1"]) {     
    ansiColor('xterm') {        
        
        sh "ansible-playbook application_tests.yml -e artefact=./target/mnt-exam.war"    
      }
    }
    sh 'cat createvm.yml'
    sh 'cat provisionvm.yml'
    sh 'cat roles/nginx/tasks/main.yml'
    sh 'cat roles/java/tasks/main.yml'
    sh 'cat roles/tomcat/tasks/main.yml'
    sh 'cat deploy.yml'
    sh 'cat application_tests.yml'
    sh 'vagrant destroy -f'
    deleteDir()
  }

}

