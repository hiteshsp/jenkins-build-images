@NonCPS
def getAffectFiles() {
    def changeLogSets = currentBuild.changeSets
    println "changeLogSets" + changeLogSets.getClass()
    
    for (int i = 0; i < changeLogSets.size(); i++) {
        def entries = changeLogSets[i].items
        println "entries" + entries.getClass()
        for (int j = 0; j < entries.length; j++) {
            def entry = entries[j]
            echo "${entry.commitId} by ${entry.author} on ${new Date(entry.timestamp)}: ${entry.msg}"
            println entry.affectedFiles.getClass()
            def files = new ArrayList(entry.affectedFiles)
            for (int k = 0; k < files.size(); k++) {
                def file = files[k]
                echo "  ${file.editType.name} ${file.path}"
            }
        }
    }
}

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
       stage('Changeset') {
           when {
                changeset comparator: 'REGEXP', pattern: '(.*Dockerfile|.*requirements\\.txt|.*\\.py)'
            }
            steps {
                script {
                   getAffectFiles()
                }
            }

       }
    }
}
