# CNTF - Main

## Purpose
This source code repository stores the configurations to create the necessary AWS infrastructure and applications (5G core & test suite) for CNTF.

## Deployment
Prerequisites:

* *Please ensure that you have configured the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-quickstart.html) to authenticate to an AWS environment where you have adequate permissions to create an EKS cluster, security groups and IAM roles* 
* *Please ensure that you have installed [Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli), [Kubernetes](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html), and [Helm](https://helm.sh/docs/intro/install/) on your local machine.*
* *Please ensure that the pipeline in this repository has been successfully deployed FIRST before executing those in other CNTF repositories. This ensures that the AWS infrastructure, 5g core, and UERANSIM test suite are available to support the execution of scripts in other repositories.*  

Steps:
1. Create a [Gitlab account](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwivrc2D7qqBAxWIlYkEHf0iBigQFnoECBQQAQ&url=https%3A%2F%2Fgitlab.com%2Fusers%2Fsign_up&usg=AOvVaw3DBJ7z7gBPTaP7wwyiUxDg&opi=89978449)
2. [Mirror](https://docs.gitlab.com/ee/user/project/repository/mirror/) this repository OR connect it [externally](https://docs.gitlab.com/ee/ci/ci_cd_for_external_repos/) to Gitlab 
3. Authenticate [Gitlab with AWS](https://docs.gitlab.com/ee/ci/cloud_deployment/)
4. Run the CI/CD pipeline (**Note:** The first deployment of the CNTF-Main pipeline may be executed by using shared Gitlab runners, which are enabled by default. For all other CNTF test runs, please set up a private Gitlab runner (details will be given in the documentation of other CNTF repositories)):
    * On the left side of the screen click the drop-down arrow next to "Build" and select "Pipelines"
    * In the top right hand corner select "Run Pipeline"
    * In the drop-down under "Run for branch name or tag" select the appropriate branch name and click "Run Pipeline"
    * Once again, click the drop-down arrow next to "Build" and select "Pipelines", you should now see the pipeline being executed
    
## Coralogix Dashboards
To view parsed & visualized data resulting from tests run by various CNTF repositories, please visit CNTF's dedicated [Coralogix tenant](https://dish-wireless-network.atlassian.net/wiki/spaces/MSS/pages/509509825/Coralogix+CNTF+Dashboards)
* Note: *You must have an individual account created by Coralogix to gain access to this tenant.*
    
Steps to view dashboards:
1. At the top of the page select the dropdown next to "Dashboards"
2. Select "Custom Dashboards" (All dashboards should have the tag "CNTF")

Raw data: To view raw data resulting from test runs, please look at the data stored in AWS S3 buckets dedicated to CNTF.

## Project Structure
```
├── coralogix_s3_setup                 contains configurations to setup s3 buckets for use by Coralogix 
|   └── README.md
|   └── s3_logs.json
|   └── s3_metrics.json
|
├── open5gs
|   ├── infrastructure                 contains infrastructure-as-code and helm configurations for open5gs & ueransim
|      	├── eks
|           └── fluentd-override.yaml  configures fluentd daemonset within the cluster
|           └── otel-override.yaml     configures opentelemtry daemonset within the cluster
|           └── provider.tf
|           └── main.tf                    
|           └── variables.tf                
|           └── outputs.tf 
|           └── versions.tf
|
└── .gitlab-ci.yml                     contains configurations to run CI/CD pipeline
|
|
└── README.md  
|
|
└── gitlab-runner-rbac.yaml            contains configurations to give gitlab runner(s) permissions to interact with kubernetes resources (manually run "kubectl apply -f gitlab-runner-rbac.yaml" on your local terminal to execute this file) 
|
|
└── ue_populate.sh                     creates one ue and subscribes it to the network, main purpose is to see if this baseline functionality works
|
|
└── ueransim_smoke_test.sh             performs a curl over both the network and internet, main purpose of this is to see if there are any network interfaces present            
                                  
```
