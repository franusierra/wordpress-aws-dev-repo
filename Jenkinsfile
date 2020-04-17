pipeline
{
    agent any
    environment
    {
        IMAGE = 'wordpress-dev:'
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
        stage('Deploy staging')
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
                    sh("docker tag $IMAGE $ECRURL:staging-$TAG")
                    docker.image("$ECRURL:staging-$TAG").push()
                    sh("docker tag $IMAGE $ECRURL:staging-latest")
                    docker.image("$ECRURL:staging-latest").push()
                }
            }
        }
        stage('Deploy production')
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
                    sh("docker tag $IMAGE $ECRURL:production-$TAG")
                    docker.image("$ECRURL:production-$TAG").push()
                    sh("docker tag $IMAGE $ECRURL:production-latest")
                    docker.image("$ECRURL:production-latest").push()
                }
            }
        }
    }

}