node('slave') {
    stage('Container Runing') {
        checkout scm
        sh "docker-compose -f /srv/compose/docker-compose-redmine-postgresql.yml ps > /tmp/ContainersRun"
        sh "cat /tmp/ContainersRun" 
        sh "rm -rf /tmp/ContainersRun"
    }
}

node('slave') {
    stage('Editing Compose Jboss') {
        sh "rm -rf /tmp/tagRedmine"
        sh "cp -R /srv/compose/ /srv/composeBKP"
        sh "cat /srv/compose/docker-compose-redmine-postgresql.yml | grep image | awk -F: 'NR==2 {print \$3}' > /tmp/tagRedmine"
        sh "sed -i 's/'\$(cat /tmp/tagRedmine)'/${params.Tag}/g' /srv/compose/docker-compose-redmine-postgresql.yml" 
        sh "cat /srv/compose/docker-compose-redmine-postgresql.yml"
        sh "rm -rf /srv/composeBKP"
    }
}

node('slave') {
    stage('Update Container') {
        input 'Deseja continuar com a ação?'

        sh "docker-compose -f /srv/compose/docker-compose-redmine-postgresql.yml up --build --no-deps -d Redmine"
        sh "rm -rf /tmp/tagRedmine"
    }
}
