pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'pip3 install -r requirement.txt'
            }
        }
        stage('Test') {
            steps {
                sh 'python3 manage.py test'
            }
        }
        
        stage('Deploy-Staging') {
            steps {
                sh 'ssh -o StrictHostKeyChecking=no deployment-user@10.150.0.5 "source venv/bin/activate; \
                cd Polling; \
                git pull origin main; \
                pip install -r requirement.txt --no-warn-script-location; \
                python3 manage.py migrate; \
                deactivate; \
                sudo systemctl restart nginx; \
                sudo systemctl restart gunicorn; "'
            }
        }
          
        stage('Deploy-Production') {
            input {
               message "Shall we deploy to Production?"
               ok "Yes Please!"
            }
            steps {
                sh 'ssh -o StrictHostKeyChecking=no deployment-user@10.150.0.2 "source venv/bin/activate; \
                cd Polling; \
                git pull origin main; \
                pip install -r requirement.txt --no-warn-script-location; \
                python3 manage.py migrate; \
                deactivate; \
                sudo systemctl restart nginx; \
                sudo systemctl restart gunicorn; "'
            }
        }
    }
}
