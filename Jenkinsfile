pipeline {
  agent {
    label 'docker'
  }

  environment {
    DOCKER_HUB = credentials('DockerHubNOAAEPIC')
    UBUNTU_GNU_IMAGE_NAME = 'ubuntu20.04-gnu9.3'
    UBUNTU_GNU_IMAGE_VERSION = '0.1'
    UBUNTU_GNU_IMAGE_TAG = "${env.DOCKER_HUB_USR}/${env.UBUNTU_GNU_IMAGE_NAME}:${env.UBUNTU_GNU_IMAGE_VERSION}"
    UBUNTU_GNU_HPCSTACK_IMAGE_NAME = "${env.UBUNTU_GNU_IMAGE_NAME}-hpc-stack"
    UBUNTU_GNU_HPCSTACK_IMAGE_VERSION = '0.1'
    UBUNTU_GNU_HPCSTACK_IMAGE_TAG = "${env.DOCKER_HUB_USR}/${env.UBUNTU_GNU_HPCSTACK_IMAGE_NAME}:${env.UBUNTU_GNU_HPCSTACK_IMAGE_VERSION}"
    UBUNTU_GNU_SRW_IMAGE_NAME = "${UBUNTU_GNU_IMAGE_NAME}-epic-srwapp"
    UBUNTU_GNU_SRW_IMAGE_VERSION = '0.1'
    UBUNTU_GNU_SRW_IMAGE_TAG = "${env.DOCKER_HUB_USR}/${env.UBUNTU_GNU_SRW_IMAGE_NAME}:${env.UBUNTU_GNU_SRW_IMAGE_VERSION}"
  }

  stages {
    stage('Build Ubuntu GNU') {
      steps {
        sh 'docker build --tag "${UBUNTU_GNU_IMAGE_TAG}" --file "${WORKSPACE}/docker/${UBUNTU_GNU_IMAGE_NAME}.docker" "${WORKSPACE}"'
      }
    }

    stage('Test Ubuntu GNU') {
      steps {
        sh 'docker run "${UBUNTU_GNU_IMAGE_TAG}" gfortran --version'
      }
    }

    stage('Release Ubuntu GNU') {
      when {
        branch 'main'
      }

      steps {
        sh 'docker login --username "${DOCKER_HUB_USR}" --password "${DOCKER_HUB_PSW}"'
        sh 'docker push "${UBUNTU_GNU_IMAGE_TAG}"'
      }
    }

    stage('Build Ubuntu GNU HPC Stack') {
      steps {
        sh 'docker build --tag "${UBUNTU_GNU_HPCSTACK_IMAGE_TAG}" --file "${WORKSPACE}/docker/${UBUNTU_GNU_HPCSTACK_IMAGE_NAME}.docker" "${WORKSPACE}"'
      }
    }

    stage('Test Ubuntu GNU HPC Stack') {
      steps {
        sh 'docker run "${UBUNTU_GNU_HPCSTACK_IMAGE_TAG}"'
      }
    }

    stage('Release Ubuntu GNU HPC Stack') {
      when {
        branch 'main'
      }

      steps {
        sh 'docker login --username "${DOCKER_HUB_USR}" --password "${DOCKER_HUB_PSW}"'
        sh 'docker push "${UBUNTU_GNU_HPCSTACK_IMAGE_TAG}"'
      }
    }

    stage('Build Ubuntu GNU SRW') {
      steps {
        sh 'docker build --tag "${UBUNTU_GNU_SRW_IMAGE_TAG}" --file "${WORKSPACE}/docker/${UBUNTU_GNU_SRW_IMAGE_NAME}.docker" "${WORKSPACE}"'
      }
    }

    stage('Test Ubuntu GNU SRW') {
      steps {
        sh 'docker run "${UBUNTU_GNU_SRW_IMAGE_TAG}"'
      }
    }

    stage('Release Ubuntu GNU SRW') {
      when {
        branch 'main'
      }

      steps {
        sh 'docker login --username "${DOCKER_HUB_USR}" --password "${DOCKER_HUB_PSW}"'
        sh 'docker push "${UBUNTU_GNU_SRW_IMAGE_TAG}"'
      }
    }
  }
}
