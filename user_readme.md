# User's guide to set up Angular CI on Jenkins from Git to S3 with tests

1. Add webhook to your github repository/branch (Our Jenkins server now: http://195.228.147.126:9090) 

2. Login to Jenkins

  - Set S3 profile
    - Go to "Manage Jenkins" --> "Configure System"
    - Find Amazon S3 profiles and set up a user (preferred to use IAM profile access key and secret key)
  - Set up a job
    - Click "New Item"
    - Choose name and "freestyle project"
  
3. Configure job
  - General field
    - choose GitHub project --> add GitHub repository url
    
  - Source Code Management field
    - Choose Git --> add clone url to repository url
    - Specifiy branches
    
  - Build Environment field
    - Check Delete workspace before build starts
    - Check Inject environment variables to the build process
      - In Properties Content you shold have at least these 2 lines
        - $ PATH=/sbin:/usr/sbin:/bin:/usr/bin:/usr/local/bin
        - $ CHROME_BIN=/opt/google/chrome/chrome
        
  - Build field
    - Choose Execute Shell
    - 3 lines:
      - npm install
      - ng test
      - ng build
      
   - Post-build Actions
   1.
    - Choose Publish JUnit test result report
      -here at Test report XMLs set the karma test results xml PATH (like **/karma-results.xml)
   2.
    - Choose Publish artifacts to S3 Bucket
      - set the previously set S+ profile
      - set destination bucket (bucket name on S3)
      - set bucket region (probably eu-central)7
      - check "No upload on build failure"

## Note:

In your karma.config file you should have the followings:
  - at plugins:
    - require('karma-junit-reporter')
    - require('karma-phantomjs-launcher')
  - at reporters:
    ['progress', 'kjhtml', 'dots', 'junit']
  - at browsers:
    - ['ChromeHeadless']
  - at singleRun:
    - true
  - also new lines for unit reporter:
    - junitReporter : {
        outputDir: 'karma-results',
        outputFile: 'karma-results.xml'
      },
        
