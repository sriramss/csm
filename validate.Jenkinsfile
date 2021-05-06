pipeline {
  agent { label "metal-gcp-builder" }

  options {
    buildDiscarder(logRotator(numToKeepStr: '10'))
    disableConcurrentBuilds()
    timestamps()
  }

  stages {
    stage('Setup Tools'){
      steps {
        sh "./validate_docker_manifests.sh install_tools"
        sh "./validate_docker_manifests.sh update_helmrepo"
      }
    }

    stage('Validate') {
      parallel {
        stage('Helm'){
          steps {
            sh "./validate_docker_manifests.sh validate_helm"
          }
        }

        stage('Containers'){
          steps {
            sh "./validate_docker_manifests.sh validate_containers"
          }
        }

        stage('Helm Versions'){
          steps {
            sh "./validate_docker_manifests.sh skopeo_sync_dry_run"
            sh "./validate_docker_manifests.sh validate_helm_images"
          }
        }

        stage('Helm Images'){
          steps {
            sh "./validate_docker_manifests.sh skopeo_sync_dry_run"
            sh "./validate_docker_manifests.sh validate_helm_images"
          }
        }
      }
    }
  }
}
