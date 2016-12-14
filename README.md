## Overview
 This project is the research part of the Service Tooling initiative of the ZHAW Service Prototyping Lab(https://blog.zhaw.ch/icclab/category/research-approach/themes/service-tooling/).
 It represents the Java translation tool for AWS Lambda. Being comparably new cloud service FaaS already became perspective area for
researching and development. AWS Lambda is one of the FaaS providers and it's the first announced service of it's kind. You can find some
information about FaaS and it's providers following the link to our recent [blog-post](https://blog.zhaw.ch/icclab/faas-function-hosting-services-and-their-technical-characteristics/).
So in common using this tool allows th e customer to make Java project available for uploading into AWS Lambda cloud service and perform automatic deploying.

## Before start
 For making the project you need maven to be installed and ${maven.home} environment variable to be set.

 For deploying Lambda functions you need to have account at AWS. Then you need to
  [install the AWS CLI](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) and
  [configure it](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html).

 Example of quick configurations:
 ```
 $ aws configure
 AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
 AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
 Default region name [None]: us-west-2
 Default output format [None]: json
 ```

## How to run
 * Checkout from repository.
 * Set up the configure files(additional/conf/)
  * Rename the "jyaml.yml.dist" file to "jyaml.yml".
  * Fill the renamed file with your configurations(if you don't need to upload functions you can leave 'aws' fields empty)
  * If your input project uses some external libraries add to 'additional/conf/pom.xml' appropriate maven dependencies.
 * Go to project location and build it using maven:
 ```
 $ mvn install
 ```
 * Run the built jar with using the 'java -jar' command. After building you can find jar in the 'target' folder inside the
  project directory. To run Podilizer you must use the following options: '-t' or '-tu' and '-conf'.
  * '-conf' - configure the path to config folder which contains jyaml.yml and pom.xml files you set up before.
  * '-t' - performs creating the new translated project and Lambda Functions building:
  ```
  java -jar target/Podilizer-0.1.jar -t -conf /path/to/conf/folder
  ```
  * '-tu' - do the same as '-t' option but additionally uploads Lambda Functions into AWS Lambda service:
  ```
  java -jar target/Podilizer-0.1.jar -tu -conf /path/to/conf/folder
  ```
  * For more information use '-help' option:
  ```
  java -jar target/Podilizer-0.1.jar -help
  ```


## Restrictions for the input project
The research is on the early stage so there are some issues to be implemented:
 * Methods of project shouldn't use 'this'.
 * All the code should be in the folder named 'src' inside the project folder('path:' in the jyaml config file).
 * Could appear bugs connected with namespaces.
 * Methods in classes that contain inner classes are not processed for the Lambda Functions. It means that such methods.
    are not separated on the different Lambda Function but still run.
 * Methods that contain one line or don't contain method body won't be translated.
 * May be there are some other restrictions that we lost and it can cause exceptions or incorrect result.
 * Supported Java version: 1.7.
