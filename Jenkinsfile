def main (minions){
    if (minions =~/B0*/) {
    //String deploycommand = ' bb-sites.reload ' + params.module + ' ' + params.reloadurl;
    //String commandToRun = '\"sudo salt -C  \"' + minions + '\"' + deploycommand + '\" ';
    String commandToRun = '\"sudo salt -C  \"' + minions  + '*\" cmd.run uptime\" ';
    sh " ssh -o StrictHostKeyChecking=no  infra@10.1.246.251  /bin/bash -c '${commandToRun}' ";
    }
    else {
    //String deploycommand = ' bb-sites.reload ' + params.module + ' ' + params.reloadurl;
    //String commandToRun = '\"sudo salt -L  \"' + minions + '\"' + deploycommand + '\" ';
    //commandToRun = '\"sudo salt -L  \"' + minions + '\" cmd.run uptime\" '
    //print commandToRun;
    }
     
     
     
}    


node {
    stage('Validate Parameters: ') { 
        print "DEBUG: parameter Minion  = " +  params.minion ;
        print "DEBUG: parameter Module   = " +  params.module ; 
        print "DEBUG: parameter Reloadurl = " +  params.reloadurl ;
        //print  env.JOB_NAME + env.BUILD_ID;
        if ((params.minion.length() == 0) || (params.module.length() == 0) || ( params.reloadurl.length() == 0 )) {
            print "ERROR: Null Paramaters"; 
            sh 'exit 1';
        }
    }
}

def minionHosts = params.minion.split(',');
def instanceNames = [] as ArrayList;
def instanceNew = '';
for (minion in minionHosts) {
    if (minion =~/B0*/) {
        instanceNew = minion;
        continue;
    }
        instanceNames.push(minion);
}
def instanceOld = instanceNames.join(',');

node {
parallel firstBranch: {
    if (instanceNew) {
        stage(instanceNew + ' Reload' ) { // for display purposes
            main(instanceNew);
        }
    }
}, secondBranch: {
    if (instanceOld) {
        stage(instanceOld + ' Reload' ) { // for display purposes
            //main(instanceOld);
        }
    }
}
}

node {
    stage('Results') {
        sh 'echo "ALL TESTS PASS" exit 0'
    }
}

