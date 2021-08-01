# golang-jenkins-pipeline
First jenkins build fora simple golang application

# Goal:
After checking in go code, jenkins shall start the build process. 
In the first steps, jenkins only have to build and test the code.

Later on, I want the code to be packaged inside a docker container and 
pushed onto the docker repository.

Furthermore it shall be deployed on k8s.

---

## GO
Simple code for a web server.
Its target via port 9000 and /hello

### GO Mod
Its now integrated and working. Compileing the program works.
I'll let the compiled program sit in this scm for the moment.

--.