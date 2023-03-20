podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: gradle
        image: gradle:jdk8
        command:
        - sleep
        args:
        - 99d
      restartPolicy: Never
    ''') {
  node(POD_LABEL) {

  stage('Start a gradle project') {
       git 'https://github.com/vijayvad/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition'
      container('gradle') {
        stage('Start Calculator') {
		sh '''
              cd Chapter09/sample1
                echo 'Start Calculator'
				curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
                chmod +x ./kubectl
                ./kubectl apply -f calculator.yaml
                ./kubectl apply -f hazelcast.yaml
         '''
       }
	   stage("Testing with curl") {
		sh '''
		cd Chapter09/sample1
		chmod +x gradlew
                ./gradlew acceptanceTest -Dcalculator.url=http://calculator-service:8080
                         '''
			publishHTML (target: [
                        reportDir: 'build/reports/tests/acceptanceTest',
                        reportFiles: 'index.html',
                        reportName: "Acceptance Report"
                    ])  
                   }
      }
    }        

  }
} 
