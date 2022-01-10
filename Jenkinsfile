pipeline{
    agent{
        docker {
            alwaysPull true
            image 'lsstts/develop-env:develop'
            args "-u root --entrypoint=''"
        }
    }
    environment {
        user_ci = credentials('lsst-io')
        LTD_USERNAME="${user_ci_USR}"
        LTD_PASSWORD="${user_ci_PSW}"
    }
    stages{
        stage("Build and Upload CCArchiver Documentation"){
            steps{
                sh """
                    source /home/saluser/.setup_dev.sh
                    pip install .
                    pip install -r doc/requirements.txt
                    package-docs build
                    ltd upload --product dm-ccarchiver --git-ref ${GIT_BRANCH} --dir doc/_build/html
                """
            }
        }
    }
    post{
       always {
            withEnv(["HOME=${env.WORKSPACE}"]) {
                sh 'chown -R 1003:1003 ${HOME}/'
            }
       }
       cleanup {
            deleteDir()
        }
    }
}
