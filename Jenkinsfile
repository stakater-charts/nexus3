#!/usr/bin/groovy
@Library('github.com/stakater/fabric8-pipeline-library@master')

String nexus3PackageName = ""
String nexus3StoragePackageName = ""
String nexus3ChartName = "nexus3"
String nexus3StorageChartName = "nexus3-storage"

clientsNode(clientsImage: 'stakater/kops-ansible:helm-bundle') {
    container(name: 'clients') {
        stage('Checkout') {
            checkout scm
        }
        
        stage('Init Helm') {
            sh "helm init --client-only"
        }

        stage('Prepare Chart') {
            nexus3PackageName = prepareChart(nexus3ChartName)
            nexus3StoragePackageName = prepareChart(nexus3StorageChartName)
        }

        stage('Upload Chart') {
            uploadChart(nexus3ChartName, nexus3PackageName)
            uploadChart(nexus3StorageChartName, nexus3StoragePackageName)
        }
    }
}

def prepareChart(String chartName) {
    result = shOutput """
                cd ${WORKSPACE}/${chartName}
                helm lint
                helm package .
            """

    return result.substring(result.lastIndexOf('/') + 1, result.length())
}

def uploadChart(String chartName, String fileName) {
    sh """
        cd ${WORKSPACE}/${chartName}
        curl -L --data-binary \"@${fileName}\" http://chartmuseum/api/charts
    """
}

def shOutput(String command) {
    return sh(
        script: """
            ${command}
        """,
        returnStdout: true).trim()
}