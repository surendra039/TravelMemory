stage('Deploy') {
    when {
        expression {
            currentBuild.currentResult == 'SUCCESS'
        }
    }
    steps {
        echo 'Deploy skipped because no staging script is available.'
    }
}
