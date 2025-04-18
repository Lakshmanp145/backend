@Library('jenkins-shared-library') _ //gets  the global pipeline libraries in system configuration

def configmap = [
    project: "expense",
    component: "backend"
]

if(! env.BRANCH_NAME.equalsIgnoreCase('main')){  //true, if branch is future branch 
    nodeJSEKSpipeline(configmap)
}
else{
    echo "Follow the process of PROD release"
}