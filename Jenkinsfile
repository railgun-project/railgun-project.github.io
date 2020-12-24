pipeline {
  agent any
  environment {
    CODING_DOCKER_REG_HOST = "${env.CCI_CURRENT_TEAM}-docker.pkg.${env.CCI_CURRENT_DOMAIN}"
    CODING_DOCKER_IMAGE_NAME = "${env.PROJECT_NAME.toLowerCase()}/${env.DOCKER_REPO_NAME}/${env.DOCKER_IMAGE_NAME}"
  }
  stages  {
    stage("检出") {
      steps {
        checkout(
          [$class: 'GitSCM',
          branches: [[name: env.GIT_BUILD_REF]],
          userRemoteConfigs: [[
            url: env.GIT_REPO_URL,
              credentialsId: env.CREDENTIALS_ID
            ]]]
        )
      }
    }  
    stage('install dep') {
      steps {
        sh "npm install"
      }
    } 
    stage('build doc') {
      steps {
        sh 'npm run docs:build'
      }
    }
    stage('deploy') {
      steps {
        sh 'cd docs/.vuepress/dist && git init && git add -A && git commit -m deploy && git push -f https://${PROJECT_TOKEN_GK}:${PROJECT_TOKEN}@e.coding.net/gsxhnd/railgun/railgun-project-cn-website.git HEAD:page'
      }
    }
  }
}