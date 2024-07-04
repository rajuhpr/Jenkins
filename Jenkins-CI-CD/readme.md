##  Deploy Java project on Docker container using Jenkins CI-CD

##### Deploy on tomcat application server: 
![image](https://github.com/rajuhpr/Jenkins/assets/142870952/c7952c7a-e421-4359-bf1f-1eed8fa54682)

#### Deploy on Docker container:

![image](https://github.com/rajuhpr/Jenkins/assets/142870952/668c9d01-1143-42f4-b6e7-62fb26f3b32e)


### Step 1: Create a Linux EC2 and install java.
  ```
    - sudo dnf update  
    - hostname Jenkin-server 
    - sudo -i
    - https://linux.how2shout.com/how-to-install-java-on-amazon-linux-2023/
    - yum install java-11*
    - ls -la
  ```
      #find installed Java using belwo command:
        - find /usr/lib/jvm/java-11* | head -n 3
            #It will show like below:
                /usr/lib/jvm/java-11-amazon-corretto.x86_64
        - java -version
        - which open JDK

      #Update Java path on .bash_profile of your root folder:
        	JAVA_HOME=/usr/lib/jvm/java-11-amazon-corretto.x86_64
        	PATH=$PATH:$HOME/bin:$JAVA_HOME
	        export PATH
      # Exit and login again to see the changes:
        - exit
        - sudo -
        - echo $JAVA_HOME
    Integrate Java on Jenkins:
         Jenkins dashboard - Manage Jenkins - myjdk
                                            - JAVA_HOME 
        
  ### Step 2: Install Jenkins
          - sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
          - sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

          -  yum install jenkins
          -  systemctl start jenkins
          -  service jenkins status
          -  systemctl enable jenkins  (automatically start when you reboot the Jenkins)
          -  ps -ef | grep jenkins to know where its running

        
  ### Step 3: Install Git 
          - yum install git
          - which git  --> Gives git location: [ /usr/bin/git ]
          
          #Install Plugins on Jenkins:
          - GitHub integration --> plugin on available plugins section on jenkins
          
          #Integrate Git on Jenkins:
          Jenkins dashboard --> Manage Jenkins
          	- name: github
          	- path : git
           
  ### Step 4: Maven Installation
          - wget https://dlcdn.apache.org/maven/maven-3/3.9.4/binaries/apache-maven-3.9.4-bin.tar.gz
          - ls
          - tar -xvzf apache-maven-3.9.4-bin.tar.gz
          - rm -r directory_name(apache-maven-3.9.4-bin.tar.gz)
          - mv apache-maven-3.9.4 maven
          - ls  (aws)
          - pwd
          - ls
          
        #I just want to keep all this software in one folder /opt.
          - cd /opt
          - ls
        #copying the maven software file in below path folder.
          - cp -r /root/maven .
          - cd maven
          - ls
          - pwd   [ /opt/maven ]
          
        #Update Maven path on .bash_profile of your root folder.
            - vi .bash_profile
          	- M2_HOME=/opt/maven  	-- taking maven from here
          	- M2=/opt/maven/bin	
          	- PATH:$M2:$M2_HOME
            - exit
            -  su - 
            - mvn --version
       #Install Maven Release Plugin from available plugin section
            
            maven integration
            
           #Integrate Maven on Jenkins:
              Jenkins dashboard - Manage Jenkins
          	  manage jenkins  --> tools --> add maven-->  
                	Name: maven
                	maven_home : /opt/maven
                     
  ### Step 4: Create a Sample Project and Verify Build (.war or .jar )
            
          Create a JOB:
          	Sample maven build:
          	   descr: maven sample project build
          	SCM: select Git
          		  paste git url on git repo ( pushing required a password, Pulling doesnt required a password as we created a global repo)
          	branch: main
          	   build step: pom.xml 
          	goal n option:
          	  - clean install package

             
          ## Verifying build on target folder
               - cd /var/lib/jenkins/workspace
               - ls
               -  cd First-Maven-Project
               -  ls
               -  cd webapp
               -  ls
               -  cd target
               - ls  
               - webapp.war -->  is generated
          #Final Build War file will be in workplace target folder
            - Below Ref Path
              /var/lib/jenkins/workspace/Sample_maven_build/webapp/target


  ### Step 5: Deploy .war build file on Tomact Apache Server

	```
 	#### Below steps are used to deploying the build file tomcat apache server

  	Create an another EC2 instanec and install Java :
   		Refer - above Step 1 for Java installation:
     	###Integrate  Jenkins with Apache Tomcat
      			Install "Deploy to container" on available plugins:
	
	#### Craete a Job :
		Integrate tomcat with jenkins:
			post build Action: select deploy war/ear to container
				- Deploy: ear/war--> **/*.war
			
				- Add container: 9x
			Then click on Add: 
				--> Jenkins
				scope --> Global(Jenkins, nodes, items) 
			
			Add username:	user: deployer
					pass: deployer
					id: user_deployer
				save It, Then
			select created jenkins user on dropdown:
			Then add:Tomact url:  IP:8080
					Ex: http://localhost:ip:8080
			
			Goto Tomcat command come out of bin:
			     cd ~
			     pwd
			     cd /opt/tomcat
			     ls
			     cd webapp
			     cd webapps
			     ls -l
			here no war file . So go and run the Job and see 
  

          
          
