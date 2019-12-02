# Setting up Nexus on AWS Linux
# SSH into Linux server
     Using putty or MobaXterm or any ssh client
# Install java
    sudo yum install java-1.8.0-openjdk -y
    
# Install Nexus 3
     cd /opt/
     sudo wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz
     
     
# Untar the file
    sudo tar xvf latest-unix.tar.gz
    
 # Change the ownership
    sudo chown -R ec2-user:ec2-user nexus-3.19.1-01 sonatype-work
    
 
# Run nexus as a service
1 Change user for nexus
    open bin/nexus.rc and make sure the following line is present
    
    run_as_user="ec2-user"

2 Execute following command
      sudo ln -s /opt/nexus-3.19.1-01/bin/nexus /etc/init.d/nexus
      
      cd /etc/init.d
      
      sudo chkconfig --add nexus
      
      sudo chkconfig --levels 345 nexus on
      
      sudo service nexus start
      
      
# Loging to Nexus3 using browser
  http:public-ip:8081
  and follow instructions to get access to nexus

# Store artifacts into nexus
We are going to use following repositories to store artifacts

Nexus settings --> repositories

maven-release

maven-snapshots

# Under Maven configure Nexus details
1. Configure nexus user/password.
Open maven settings($MAVEN_HOME/conf/settings.xml) file and add following snippet

       <servers>
        <server>
          <id>nexusRepo</id>
          <username>admin</username>
          <password>admin</password>
        </server>
      </servers>
  
  
2. Configure pom.xml of your project
  Make sure the following snippet exists in pom.xml

    <distributionManagement>
         <snapshotRepository>
         
              <id>nexusRepo</id>
              
              <url>http://13.233.230.166:8081/repository/maven-snapshots/</url>
              
         </snapshotRepository>
          <repository>
          
            <id>nexusRepo</id>
            
            <url>http://13.233.230.166:8081/repository/maven-releases/</url>
            
          </repository>
          
      </distributionManagement>
