pipeline {
  agent any
    
  stages {
    stage('update main page'){
      input {
        message "What is your main image tag?"
        ok "finished"
        parameters {
          string(name: 'TAG1', defaultValue: 'none', description: 'input your tag')
        }
      }
      steps {
        sh '''
        sudo ansible kvm1 -m shell -a 'rm -rf cicd &&
        git clone https://github.com/ingi-h/cicd.git &&
        cd cicd &&
        docker build -t rudclthe/testimg:${TAG1} main/ &&
        docker push rudclthe/testimg:$(TAG1) &&
        kubectl set image deployment deploy-main ctn-main=rudclthe/testimg:${TAG1} -n company1' -e "TAG1=${TAG1}" --become
        '''
      }
    }

    stage('update blog page'){
      input {
        message "What is your blog image tag?"
        ok "finished"
        parameters {
          string(name: 'TAG2', defaultValue: 'none', description: 'input your tag')
        }
      }
      steps {
        sh '''
        sudo ansible kvm1 -m shell -a 'docker build -t rudclthe/testimg:${TAG2} blog/ &&
        docker push rudclthe/testimg:$(TAG2) &&
        kubectl set image deployment deploy-blog ctn-blog=rudclthe/testimg:${TAG2} -n company1' -e "TAG2=${TAG3}" --become
        '''
      }
    }

    stage('update shop page'){
      input {
        message "What is your shop image tag?"
        ok "finished"
        parameters {
          string(name: 'TAG3', defaultValue: 'none', description: 'input your tag')
        }
      }
      steps {
        sh '''
        sudo ansible kvm1 -m shell -a 'docker build -t rudclthe/testimg:${TAG3} shop/ &&
        docker push rudclthe/testimg:$(TAG3) &&
        kubectl set image deployment deploy-shop ctn-shop=rudclthe/testimg:${TAG3} -n company1' -e "TAG3=${TAG3}" --become
        '''
      }
    }

  }
  post {
    always {
      echo "작업완료"
    }
  }
}


