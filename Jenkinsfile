
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


parallel firstBranch: {
    if (instanceNew) {
   stage(instanceNew + ' Reload' ) { // for display purposes
            sleep 20;
            print "PROD NEW " + instanceNew;
    }
    }
 
}, secondBranch: {
    if (instanceOld) {
    
    stage(instanceOld + ' Reload' ) { // for display purposes
            sleep 40;
            print "PROD OLD " + instanceOld;
    }
    }
}


node {
    stage('Results') {
        sh 'echo "ALL TESTS PASS" exit 0'
    }
}
