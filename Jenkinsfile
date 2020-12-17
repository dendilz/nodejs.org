node('slave') {
    git url: 'https://github.com/dendilz/nodejs.org.git'
    env.GIT_COMMIT_NAME = getCommit()
    env.APP_PATH = "/var/www"
    
    if(!fileExists("$APP_PATH/site-$GIT_COMMIT_NAME")){
        stage ('Build'){
            sh "npm install"
            sh "npm run build"
        }
        stage ('Deploy'){
            sh "mkdir $APP_PATH/site-$GIT_COMMIT_NAME"
            sh "cp -r build/* $APP_PATH/site-$GIT_COMMIT_NAME"
            sh "ln -fsn $APP_PATH/site-$GIT_COMMIT_NAME $APP_PATH/site"
        }
        stage ('Remove old builds'){
            sh "cd $APP_PATH/ && ls -tp | tail -n +5 | xargs rm -rf"
        }
        stage ('Save artifacts') {
            archiveArtifacts artifacts: 'build/**/*.*', fingerprint: true
        }
    }    
}

String getCommit() {
    return sh(script: """git rev-parse HEAD | tail -c 7""", returnStdout: true)?.trim()
}
