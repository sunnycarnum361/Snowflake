pipeline {
    options {
      timeout(time: 1, unit: 'HOURS') 
  }
  agent {
    environment {
        PATH = "/usr/local/bin:${env.PATH}"
      }
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
}
