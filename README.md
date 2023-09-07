# tomcat install on Amazon Linux EC2 instance, enable manager app, create a login

# Install Java as prerequisite to tomcat
yum install java -y

# Install utilities just in case you don't find them
yum install wget vim zip unzip -y
cd /opt

# Download tomcat installables from apache official site (Google 'download tomcat')
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.80/bin/apache-tomcat-9.0.80.zip
ls -lrt

# Unzip the downloaded file
unzip apache-tomcat-9.0.80.zip
ls
cd apache-tomcat-9.0.80/bin # ===> Where you find the start stop executables
ls -lrt

# To make them executables
chmod u+x *
ls -lrt
pwd

# Create Symlinks to startup.sh and shutdown.sh to call them from outside this directories.
ln -s /opt/apache-tomcat-9.0.80/bin/startup.sh /usr/bin/tomcatup
ln -s /opt/apache-tomcat-9.0.80/bin/shutdown.sh /usr/bin/tomcatdown

# Start tomcat
tomcatup
ps -afx | grep tomcat # ===> You should find processes showing tomcat
netstat -tunlp # ===> Find the default port 8080 where tomcat is listening on.

# Configure to change tomcat port
cd /opt/apache-tomcat-9.0.80/conf/
ls
cp server.xml server.xml.old
vi server.xml # ===> Change the ports from 8080 to whichever
tomcatdown; tomcatup ===> Restart tomcat
 
# This is to enable Manager App in Tomcat server
cd to /opt/apache-tomcat-9.0.80/webapps/manager/META-INF
vi context.xml # ===> Comment out 4 lines shown in the video

# Create login access
/opt/apache-tomcat-9.0.80/conf
vi tomcat-users.xml
