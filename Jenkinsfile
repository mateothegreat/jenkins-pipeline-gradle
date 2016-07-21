#!groovy
stage 'Build'
node ('master') {
    git url 'https://github.com/mateothegreat/jenkins-pipeline-gradle.git'
    mvn 'clean package'
    dir('target') {stash name: 'war', includes: 'x.war'}
}
stage 'Test'
parallel(long: {
    runTests(30)
}, quick: {
    runTests(20)
})
stage name: 'Deploy', concurrency: 1
node ('master') {
    deploy 'remote'
}
def mvn(args) {
    sh "${tool 'Maven 3.x'}/bin/mvn ${args}"
}
def runTests(duration) {
    node {
        sh "sleep ${duration}"
    }
}
def deploy(id) {
    unstash 'war'

    sh "cp x.war /tmp/${id}.war"

}

def undeploy(id) {

    sh "rm /tmp/${id}.war"
}
