node {
    def app

    stage('Clone repository') {
        /* Cloning the Repository to our Workspace */

        checkout scm
    }
    environment {
      PATH = "/usr/local/bin:${env.PATH}"
    }
    stage('Build image') {
        /* This builds the actual image */

        app = docker.build("hashmapinc/sqitch")
    }

    stage('Test image') {

        app.inside {
            echo "Tests passed"
        }
    }

    stage('Pull image') {
        /*
			You would need to first register with DockerHub before you can push images to your account
		*/
        docker.withRegistry('https://registry.hub.docker.com', 'docker_hub') {
            /*app.push("${env.BUILD_NUMBER}")*/
            app.pull("snowflake-dev")
            }
                echo "Trying to Pull Docker Build to DockerHub"
    }
}



/*pipeline {
    options {
      timeout(time: 1, unit: 'HOURS') 
  }
  agent {
    docker {
      docker.withRegistry('https://registry.hub.docker.com', 'docker_hub') {
      docker.image('hashmapinc/sqitch:snowflake-dev').pull()
    }
  }
  stages {
    stage('Installing Latest snowsql') {
        steps {
            sh 'snowsql --help'
        }
    }
    stage('Deploy changes') {
      steps {
        withCredentials(bindings: [usernamePassword(credentialsId: 'snowflake_creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          sh '''
              sqitch deploy "db:snowflake://$USERNAME:$PASSWORD@es10367.eu-west-1.snowflakecomputing.com/flipr?Driver=Snowflake;warehouse=sqitch_wh"
              '''           
        }
      }
    }
    stage('Verify changes') {
      steps {
        withCredentials(bindings: [usernamePassword(credentialsId: 'snowflake_creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          sh '''
              sqitch verify "db:snowflake://$USERNAME:$PASSWORD@es10367.eu-west-1.snowflakecomputing.com/flipr?Driver=Snowflake;warehouse=sqitch_wh"
              ''' 
        }
      }
    }      
  }  
post {
    always {
      sh 'chmod -R 777 .'
    }
  }
} */
