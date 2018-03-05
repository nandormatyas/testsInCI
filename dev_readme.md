# This devops guide is to set up CI process from github to AWS S3 with testings for Angular projects

## Jenkins plugins setup
- Sign in to Jenkins
- Manage Jenkins --> Manage Plugins
  - search for s3 --> choose Amazon S3 Bucket Credentials Plugin and S3 publisher plugin --> install
  - search for junit --> choose JUnit Attachments Plugin and JUnit Realtime Test Reporter Plugin --> install
  
## Create testing evironment for Angular projects
  - connect to the server through ssh
  - connect to the docker image by typing sudo docker exec -it <container name> bash
    - install nodejs and npm:
    
      - curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash -      
      - sudo apt-get install -y nodejs      
      - sudo apt-get install -y build-essential    
      
    - install Angular:
      - npm install -g @angular/cli
      
    - install test environment:
      - npm install -g karma-cli
      - npm install -g karma --save-dev
      - npm install -g phantomjs 
      - npm install -g karma-jasmine --save-dev 
      - npm install -g karma-phantomjs-launcher --save-dev
      - npm install -g karma-coverage
      
    - install chrome and/or other browsers:
       - now covering Chrome:
         - wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
         - sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
         - apt-get update && apt-get install -y google-chrome-stable
       - since Angular tests are running in the browsers, but we don't have ui on the server we have to run browsers in headless mode
         - navigate to the folder you installed Chrome (like /opt/google/chrome/) 
         - open google-chrome with vim
         - Edit the file /opt/google/chrome/google-chrome
            - find line exec -a "$0" "$HERE/chrome" "$@"
            - edit it like this DISPLAY=:0 exec -a "$0" "$HERE/chrome" "$0" --headless --no-sandbox
          
