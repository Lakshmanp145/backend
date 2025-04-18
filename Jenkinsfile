@Library('jenkins-shared-library') _

def configmap = [
    project: "expense",
    component: "backend"
]

if(! env.BRANCH_NAME.equalsIgnoreCase('main')){
    nodeJSEKSpipeline(configmap)
}
else{
    echo "Follow the process of PROD release"
}