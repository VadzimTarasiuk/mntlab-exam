node("${env.SLAVE}") {

  stage("Build"){
    git branch: 'vtarasiuk', url: 'https://github.com/VadzimTarasiuk/mntlab-exam.git'
    sh  'echo "Build time = $(date)\nBuild Machine Name = $(hostname)\nBuild User Name = vtarasiuk\nGIT URL: ${GIT_URL}\nGIT Commit: ${GIT_COMMIT}\nGIT Branch: ${GIT_BRANCH}" > ./src/main/resources/build-info.txt'
    sh '/home/student/apache-maven-3.5.0/bin/mvn clean package -DbuildNumber=$BUILD_NUMBER'
    sh "echo build artefact"
  }

  stage("Package"){
    /*
        use tar tool to package built war file into *.tar.gz package
    */
    sh "echo package artefact"
  }

  stage("Roll out Dev VM"){
    /*
        use ansible to create VM (with developed vagrant module)
    */
    sh "echo ansible-playbook createvm.yml ..."
  }

  stage("Provision VM"){
    /*
        use ansible to provision VM
        Tomcat and nginx should be installed
    */
    sh "echo ansible-playbook provisionvm.yml ..."
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
    sh "echo ansible-playbook deploy.yml -e artefact=... ..."
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
    sh "echo ansible-playbook application_tests.yml -e artefact=... ..."
  }

}

