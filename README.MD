run a ubuntu docker image and loing to the container then executhe the below commaand
apt-get update -y
apt install openjdk-8-jdk -y
apt install maven -y
apt install vim -y
apt install tree -y
apt install wget -y
apt install unzip -y 
#java installed path /usr/lib/jvm/java-8-openjdk-amd64/bin/java
#maven installed path /usr/bin/mvn
#Create mvn webapps project refer to the url https://c4clouds.com/build-a-sample-helloworld-war-with-maven/
mvn clean package

#install apache tomcat to deploy the war webapp
cd /tmp
wget http://mirrors.estointernet.in/apache/tomcat/tomcat-8/v8.5.46/bin/apache-tomcat-8.5.46.zip
unzip apache-tomcat-8.5.46.zip
mv apache-tomcat-8.5.46 tomcat
mv tomcat /usr/local/

#copy the war file to webapp
#webapp home location /usr/local/tomcat/webapps
cp target/hello-world-war-1.0.1.war /usr/local/tomcat/webapps/hello-world.war
sh /usr/local/tomcat/bin/startup.sh


#Docker multistage build

Create a Docker file and apend the blow lines 
FROM maven:3.6.2-jdk-8 as builder
COPY . /
RUN mvn clean package

FROM tomcat:8.0
COPY --from=builder /target/*.war /usr/local/tomcat/webapps/hello.war
EXPOSE 8080
WORKDIR /usr/local/tomcat/bin/
CMD ["catalina.sh", "run"]

# build the docker image and compare both the images size
#Now you can further reduce the image size by changing the Image name FROM tomcat:8.0 to FROM tomcat:8.0-alpine 
