pipeline {
    agent any
   tools { 
        maven 'Maven 3' 
        jdk 'jdk8' 
    }
    parameters {
        booleanParam(name: "RELEASE",
                description: "Build a release from current commit.",
                defaultValue: false)
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }

        stage('Build') {
            steps {
               echo 'This is a minimal pipeline.'
               sh 'mvn clean package'
               /*docker {
                  args '-p 9000:9000 --name snapshot'
                   image '${JOB_NAME}:SNAPSHOT'
                }*/
               /* script{
                    app = docker.build("${JOB_NAME}:SNAPSHOT").with
                }*/

            }
        }
        
        stage('Build Image'){
           /* agent {
    // Equivalent to "docker build -f Dockerfile.build --build-arg version=1.0.2 ./build/
         dockerfile {
         filename 'Dockerfile'
         dir '.'
            //label 'my-defined-label'
            //additionalBuildArgs  '--build-arg version=1.0.2'
            //args '-p 9000:9000 -t snapshot'
    }
}*/
            steps{
                script{
                    def snapshotImage = docker.build("${JOB_NAME}:${env.BUILD_ID}")
                }
                
            }
        
        }
         stage('Release') {
        
                when {
                expression { params.RELEASE }
            }
            steps {
                script{
                    withMaven(maven: 'Maven 3'){
                        withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'password', usernameVariable: 'username')])
                             sh "git config user.email ecsdigitalpune@gmail.com && git config user.name Jenkins"
                             sh "mvn release:prepare release:perform -Dusername=${username} -Dpassword=${password}"
                    }
                }
                
            }
               /* steps{

                script{
             withMaven(maven: 'Maven 3') {
          //releasedVersion = getReleasedVersion()
          withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'password', usernameVariable: 'username')]) {
              sh "git config user.email ecsdigitalpune@gmail.com && git config user.name Jenkins"
              sh "mvn release:prepare release:perform -Dusername=${username} -Dpassword=${password}"
          }
          //docker "build --tag digitaldemo-docker-release-images.jfrog.io/sparktodo-${JOB_NAME}:${releasedVersion} ."
     // }
  }}
  }*/

    }
}
}
