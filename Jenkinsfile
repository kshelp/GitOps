pipeline {
  agent any
  stages {
    stage('git pull') {
      steps {
        // Git 저장소에서 코드를 가져옵니다.
        // https://github.com/kshelp/GitOps.git 주소는 필요에 따라 변경될 수 있습니다.
        git url: 'https://github.com/kshelp/GitOps.git', branch: 'main'
      }
    }
    stage('k8s deploy') {
      steps {
        // Jenkins Credentials (kubeconfigId: 'kubeconfig')를 사용하여 kubeconfig 파일을 임시로 생성합니다.
        // 이렇게 하면 kubeconfig 내용이 로그에 직접 노출되지 않고 안전하게 사용될 수 있습니다.
        withCredentials([kubeconfigFile(credentialsId: 'kubeconfig', variable: 'KUBECONFIG_FILE')]) {
          sh '''
            # 현재 작업 디렉토리의 모든 .yaml 파일을 kubectl apply합니다.
            # KUBECONFIG 환경 변수를 설정하여 kubectl이 올바른 kubeconfig 파일을 사용하도록 합니다.
            # -f . 옵션은 현재 디렉토리의 모든 YAML 파일을 찾아서 적용합니다.
            KUBECONFIG=${KUBECONFIG_FILE} kubectl apply -f .
          '''
        }
      }
    }
  }
}
