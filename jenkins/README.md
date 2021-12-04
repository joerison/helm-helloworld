# Jenkins
## Install with Kubernetes (recommended)
    kubectl create namespace jenkins
    kubectl apply -f jenkins.yaml --namespace jenkins 

## Configuration
Go Jenkins http://localhost:{JENKINS_PORT}/pluginManager/available

Search `docker` and `kubernetes` and install it.

## Grant permission
If you receive this message:

    Error testing connection : Failure executing: GET at: https://10.96.0.1/api/v1/namespaces/jenkins/pods. Message: 
    Forbidden!Configured service account doesn't have access. Service account may have been revoked. pods is forbidden: 
    User "system:serviceaccount:jenkins:default" cannot list resource "pods" in API group "" in the namespace "jenkins".
Run this command:

    kubectl create clusterrolebinding jenkins-default-admin --clusterrole cluster-admin --serviceaccount=default:default

## Install (others options)
### Install with Docker
#### (Issues to connect with kubernetes)
    docker run --name jenkins-docker --rm --detach \
    --privileged --network jenkins --network-alias docker \
    --env DOCKER_TLS_CERTDIR=/certs \
    --volume jenkins-docker-certs:/certs/client \
    --volume jenkins-data:/var/jenkins_home \
    --publish 2376:2376 docker:dind --storage-driver overlay2
    
    docker run --name jenkins-blueocean --rm --detach \
    --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
    --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
    --publish 8084:8080 --publish 50000:50000 \
    --volume jenkins-data:/var/jenkins_home \
    --volume jenkins-docker-certs:/certs/client:ro \
    jenkinsci/blueocean

Reference: https://www.jenkins.io/doc/book/installing/docker/

### Install with brew
    brew install jenkins-lts
    brew services restart jenkins-lts
    brew services stop jenkins-lts

#### Configure (OSX)
Will allow OS environment variables to be shared with Jenkins.

Put this code inside `/usr/local/Cellar/jenkins-lts/2.303.3/homebrew.mxcl.jenkins-lts.plist`

	<array>
		<string>/usr/local/opt/openjdk@11/bin/java</string>
		<string>-Dmail.smtp.starttls.enable=true</string>
		<string>-jar</string>
		<string>/usr/local/opt/jenkins-lts/libexec/jenkins.war</string>
		<string>--httpListenAddress=127.0.0.1</string>
		<string>--httpPort=8080</string>
	</array>
	<key>RunAtLoad</key>
	<true/>
	<key>EnvironmentVariables</key>
    <dict>
      <key>PATH</key>
      <string>/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Applications/Docker.app/Contents/Resources/bin</string>
    </dict>
