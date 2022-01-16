pipeline {
    agent any

     environment {
        dotnet ='C:\\Program Files (x86)\\dotnet\\dotnet.exe'
        powershell ='C:\\Program Files\\PowerShell\\7\\pwsh.exe'
        }

 
    stages {
       
        stage('Restore packages'){
           steps{
               bat 'dotnet restore eShopOnWeb.sln'
            }
         }
        stage('Clean'){
           steps{
              bat 'dotnet clean eShopOnWeb.sln --configuration Release'
            }
         }
        stage('Build'){
           steps{
               echo 'building the application'
               bat 'dotnet build eShopOnWeb.sln --configuration Release --no-restore'
            }
         }
        stage('Increase version') {
    steps {
        echo "${env.BUILD_NUMBER}"
        powershell '''
           $xmlFileName = "eShopOnWeb_main\\Package.appxmanifest"     
           [xml]$xmlDoc = Get-Content $xmlFileName
           $version = $xmlDoc.Package.Identity.Version
           $trimmedVersion = $version -replace '.[0-9]+$', '.'
           $xmlDoc.Package.Identity.Version = $trimmedVersion + ${env:BUILD_NUMBER}
           echo 'New version:' $xmlDoc.Package.Identity.Version
           $xmlDoc.Save($xmlFileName)
        '''
     }
 }
        stage('Test: Unit Test'){
           steps {
               echo 'Unit Test'
                bat 'dotnet test tests/UnitTests/UnitTests.csproj --configuration Release --no-restore'
             }
          }

            stage('Test: Integration Test'){
           steps {
               echo 'Integration Test'
                bat 'dotnet test tests/IntegrationTests/IntegrationTests.csproj --configuration Release --no-restore'
             }
          }

           stage('Test: Functional Test'){
           steps {
               echo 'Functional Test'
                bat 'dotnet test tests/FunctionalTests/FunctionalTests.csproj --configuration Release --no-restore'
             }
          }

        stage('Publish'){
             steps{
               bat 'dotnet publish src/Web/Web.csproj --configuration Release --no-restore'
             }
        }
        stage('Deploy'){
             steps{
                echo 'deploying the application'
             }
        }
    }
}
