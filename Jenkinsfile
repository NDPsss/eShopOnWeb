pipeline {
    agent any

     environment {
        dotnet ='C:\\Program Files (x86)\\dotnet\\dotnet.exe'
        }

    triggers {
        githubPush()
    }
    stages {
        stage('Restore packages'){
           steps{
               sh 'dotnet restore eShopOnWeb.sln'
            }
         }
        stage('Clean'){
           steps{
               sh 'dotnet clean eShopOnWeb.sln --configuration Release'
            }
         }
        stage('Build'){
           steps{
               echo 'building the application'
               sh 'dotnet build eShopOnWeb.sln --configuration Release --no-restore'
            }
         }
        stage('Test: Unit Test'){
           steps {
               echo 'Unit Test'
                sh 'dotnet test UnitTests/UnitTests.csproj --configuration Release --no-restore'
             }
          }

            stage('Test: Integration Test'){
           steps {
               echo 'Integration Test'
                sh 'dotnet test IntegrationTests/IntegrationTests.csproj --configuration Release --no-restore'
             }
          }

           stage('Test: Functional Test'){
           steps {
               echo 'Functional Test'
                sh 'dotnet test FunctionalTests/FunctionalTests.csproj --configuration Release --no-restore'
             }
          }

        stage('Publish'){
             steps{
               sh 'dotnet publish eShopOnWeb/eShopOnWeb.csproj --configuration Release --no-restore'
             }
        }
        stage('Deploy'){
             steps{
                echo 'deploying the application'
             }
        }
    }
}
