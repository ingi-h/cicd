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
        sudo ansible kvm1 -m file -a "path=/root/cicd state=absent"
        sudo ansible kvm1 -m shell -a 'git clone https://github.com/ingi-h/cicd.git &&
        cd cicd &&
        docker build -t rudclthe/testimg:{{TAG}} main/ &&
        docker push rudclthe/testimg:{{TAG}} &&
        kubectl set image deployment deploy-main ctn-main=rudclthe/testimg:{{TAG}} -n company1' -e "TAG=${TAG1}" --become
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
        sudo ansible kvm1 -m shell -a 'docker build -t rudclthe/testimg:{{TAG}} blog/ &&
        docker push rudclthe/testimg:{{TAG}} &&
        kubectl set image deployment deploy-blog ctn-blog=rudclthe/testimg:{{TAG}} -n company1' -e "TAG=${TAG2}" --become
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
        sudo ansible kvm1 -m shell -a 'docker build -t rudclthe/testimg:{{TAG}} shop/ &&
        docker push rudclthe/testimg:{{TAG}} &&
        kubectl set image deployment deploy-shop ctn-shop=rudclthe/testimg:{{TAG}} -n company1' -e "TAG=${TAG3}" --become
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

