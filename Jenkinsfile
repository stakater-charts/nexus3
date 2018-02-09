#!/usr/bin/groovy
@Library('github.com/stakater/fabric8-pipeline-library@master')

String nexus3PackageName = ""
String nexus3StoragePackageName = ""
String nexus3ChartName = "nexus3"
String nexus3StorageChartName = "nexus3-storage"

clientsNode(clientsImage: 'stakater/kops-ansible:helm-bundle') {
    container(name: 'clients') {
        def helm = new io.stakater.charts.Helm()
        def chartManager = new io.stakater.charts.ChartManager()
        stage('Checkout') {
            checkout scm
        }
        
        stage('Init Helm') {
            helm.init(true)
        }

        stage('Prepare Chart') {
            helm.lint(WORKSPACE, nexus3ChartName)
            nexus3PackageName = helm.package(WORKSPACE, nexus3ChartName)

            helm.lint(WORKSPACE, nexus3StorageChartName)
            nexus3StoragePackageName = helm.package(WORKSPACE, nexus3StorageChartName)
        }

        stage('Upload Chart') {
            chartManager.uploadToChartMuseum(WORKSPACE, nexus3ChartName, nexus3PackageName)
            chartManager.uploadToChartMuseum(WORKSPACE, nexus3StorageChartName, nexus3StoragePackageName)
        }
    }
}
