pipeline
{
    agent any
    environment
    {
        IMAGE = 'wordpress-dev'
        ECRURL = '531104511012.dkr.ecr.eu-west-1.amazonaws.com/wordpress-dev'
    }
    stages
    {
        stage('Build')
        {
            when{
                anyOf{
                    branch 'master'
                    branch 'develop'
                }
            }
            steps
            {
                script
                {
                    echo "Begin "+env.BRANCH_NAME+" build..."
                    TAG = "${GIT_COMMIT[1..7]}"
                    docker.build("$IMAGE:$TAG",".")
                }
            }
        }
        stage('Push staging image')
        {
            when{
                branch 'develop'
            }

            steps
            {
                script
                {
                    echo "Begin "+env.BRANCH_NAME+" build..."
                    sh("eval \$(aws ecr get-login --region eu-west-1 --no-include-email)")
                    sh("docker tag $IMAGE:$TAG $ECRURL:staging-$TAG")
                    docker.image("$ECRURL:staging-$TAG").push()
                    sh("docker tag $IMAGE:$TAG $ECRURL:staging-latest")
                    docker.image("$ECRURL:staging-latest").push()
                }
            }
        }
        stage('Run deployment staging job')
        {
            when{
                branch 'develop'
            }

            steps
            {
                script
                {
                    echo "Running deployment staging job..."
                    build job:'Wordpress-Ops/master',
                        wait: false,
                        parameters: [
                            string(name: 'IMAGE',value: "$ECRURL:staging-$TAG"),
                            string(name: 'ENVIRONMENT',value: "staging")
                        ]
                }
            }
        }
        stage('Push production image')
        {
            when{
                branch 'master'
            }
            steps
            {
                script
                {
                    echo "Begin "+env.BRANCH_NAME+" build..."
                    sh("eval \$(aws ecr get-login --region eu-west-1 --no-include-email)")
                    sh("docker tag $IMAGE:$TAG $ECRURL:production-$TAG")
                    docker.image("$ECRURL:production-$TAG").push()
                    sh("docker tag $IMAGE:$TAG $ECRURL:production-latest")
                    docker.image("$ECRURL:production-latest").push()
                }
            }
        }
        stage('Run deployment production job')
        {
            when{
                branch 'master'
            }

            steps
            {
                script
                {
                    echo "Running deployment production job..."
                    build job:'Wordpress-Ops/master',
                        wait: false,
                        parameters: [
                            string(name: 'IMAGE',value: "$ECRURL:production-$TAG"),
                            string(name: 'ENVIRONMENT',value: "production")
                        ]
                }
            }
        }
    }

}