# jenkins-pipeline

### Timeout & WaitUntil

[Solution](https://stackoverflow.com/questions/46978924/how-to-test-sh-script-return-status-with-jenkins-declarative-pipeline-new-syntax)

```sh
stage('Check Server') {
    steps {

        timeout(10){ // 10 mins
            script {
                // waitUntil: Wait for condition
                //      1. Runs its body repeatedly until it returns true. If it returns false, waits a while and tries again.
                //      2. There is no limit to the number of retries, but if the body throws an error that is thrown up immediately.
                waitUntil {
                    //  Monitoring container whether database container is ready.
                    //  If not, sleep 1 min.
                   
                    def output = sh(returnStdout: true, script: "docker logs server").trim();
                    if (output.contains("hello world"))
                    {
                        return true
                    } else {
                        echo 'server is not ready.'
                        sleep(time:1,unit:"MINUTES")
                        return false
                    }
                }
            }
        }
    }
}

```

