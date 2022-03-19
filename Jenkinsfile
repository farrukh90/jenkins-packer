properties([parameters([string(defaultValue: '713287746880', description: 'Please provide your AWS account ', name: 'AWS_ACCOUNT', trim: true)])])

node {
    def REPOLOCATION = "https://github.com/farrukh90/jenkins-packer.git"
    def BRANCHNAME = "main"
    
    stage("Clean Up") {
        sh "docker images"
        sh "docker image prune --force"
    }
    stage("Pull Docker Image"){
        sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${AWS_ACCOUNT}.dkr.ecr.us-east-1.amazonaws.com"
        sh "docker pull ${AWS_ACCOUNT}.dkr.ecr.us-east-1.amazonaws.com/tools:latest"
    }
    withDockerContainer(image: '${AWS_ACCOUNT}.dkr.ecr.us-east-1.amazonaws.com/tools:latest'){
        stage("Clone") {
            checkout([$class: 'GitSCM', branches: [[name: "*/${BRANCHNAME}"]], extensions: [], userRemoteConfigs: [[url: "${REPOLOCATION}"]]])
        }
    }
    withDockerContainer(image: 'hashicorp/packer:1.7.10'){
        stage("Initialize") {
            sh "packer init  ."
        }
    }
    withDockerContainer(image: 'hashicorp/packer:1.7.10'){
        stage("Validate") {
            sh "packer validate  ."
        }
    }
    withDockerContainer(image: 'hashicorp/packer:1.7.10'){
        stage("Apply") {
            sh "packer build image.pkr.hcl"
        }
    }
    stage("Email notification") {
        sh "echo hello"
    }
}



// node {
//     Authenticate, 
//     pull docker image
//      create container
//              Clone a repo
//      create container     
//              terraform init
//      create vault container
//              pull secrets
//      create container 
//              terraform apply
//      create container 
//              run packer
//      create sonarqube container
//              run sonarqube tasks
// }







// node {
//     Clone 
//     Code analysis   (sonarqube, veracode)
//     Build 
//     Tagging
//     Authentication 
//     Push 
//     Email 
// }