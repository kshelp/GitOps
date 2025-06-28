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
        // 'kubeconfig'라는 ID로 등록된 kubeconfig Credentials를 사용합니다.
        // 이 스텝은 KUBECONFIG 환경 변수를 자동으로 설정해 줍니다.
        // Kubeconfig Credentials 등록 시 Kind는 "Kubernetes configuration (kubeconfig)"으로 설정되어야 합니다.
        withKubeConfig(credentialsId: 'kubeconfig') { // <-- 'kubeconfigFile' 대신 'withKubeConfig' 사용
          sh '''
            # KUBECONFIG 환경 변수는 이미 withKubeConfig 스텝에 의해 설정되어 있습니다.
            # 현재 작업 디렉토리의 모든 .yaml 파일을 kubectl apply합니다.
            kubectl apply -f .
          '''
        }
      }
    }
  }
}
