# Step 1: Create an Amazon ES Domain<a name="es-gsg-create-domain"></a>

**Important**  
This process is a concise tutorial for configuring a *test domain*\. It should not be used to create production domains\. For a comprehensive version of the same process, see [[ERROR] BAD/MISSING LINK TEXT](es-createupdatedomains.md)\.

An Amazon Elasticsearch Service \(Amazon ES\) domain encapsulates the Elasticsearch engine instances that process HTTP requests to AWS, the indexed data that you want to search, snapshots of the domain, access policies, and metadata\. You can create an Amazon ES domain by using the Amazon ES console, the AWS CLI, or the AWS SDK\. If you don't already have an account, see [[ERROR] BAD/MISSING LINK TEXT](what-is-amazon-elasticsearch-service.md#aws-sign-up)\.

**To create an Amazon ES domain \(console\)**

1. Go to [https://aws\.amazon\.com](https://aws.amazon.com), and then choose **Sign In to the Console**\.

1. Under **Analytics**, choose **Elasticsearch Service**\.

1. On the **Define domain** page, for **Elasticsearch domain name**, type a name for the domain\. In this Getting Started tutorial, we use the domain name *movies* for the examples that we provide later in the tutorial\.

1. For **Version**, choose an Elasticsearch version for your domain\. We recommend that you choose the latest supported version\. For more information, see [[ERROR] BAD/MISSING LINK TEXT](what-is-amazon-elasticsearch-service.md#aes-choosing-version)\.

1. Choose **Next**\.

1. For **Instance count**, choose the number of instances that you want\. For this tutorial, you can use the default value of 1\.

1. For **Instance type**, choose an instance type for the Amazon ES domain\. For this tutoral, we recommend `t2.small.elasticsearch`, a small and inexpensive instance type suitable for testing purposes\.

1. For now, you can ignore the **Enable dedicated master** and **Enable zone awareness** check boxes\. For more information about both, see About Dedicated Master Nodes and Enabling Zone Awareness\.

1. For **Storage type**, choose **EBS**\.

   The **EBS volume type** and **EBS volume size** boxes appear\.

   1. For **EBS volume type**, choose General Purpose \(SSD\)\. For more information, see [Amazon EBS Volume Types\.](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html)

   1. For **EBS volume size**, type the size in GB of the external storage for *each* data node\. For this tutorial, you can use the default value of 10\.

1. For now, you can ignore **Enable encryption at rest**\. For more information about the feature, see [[ERROR] BAD/MISSING LINK TEXT](encryption-at-rest.md)\.

1. For **Automated snapshot start hour**, use the default value\. For more information, see [[ERROR] BAD/MISSING LINK TEXT](es-createupdatedomains.md#es-createdomain-configure-snapshots)\.

1. Choose **Next**\.

1. For simplicity in this tutorial, we'll use an IP\-based access policy\. On the **Set up access** page, in the **Network configuration** section, choose **Public access**\.

1. For **Set the domain access policy to**, choose **Allow access to the domain from specific IP\(s\)** and enter your public IP address, which you can find by searching for "What is my IP?" on [Google](https://www.google.com)\. Then choose **OK**\.

   To learn more about public access, VPC access, and access policies in general, see [[ERROR] BAD/MISSING LINK TEXT](es-ac.md) and [[ERROR] BAD/MISSING LINK TEXT](es-vpc.md)\.

1. Choose **Next**\.

1. On the **Review** page, review your domain configuration, and then choose **Confirm**\.
**Note**  
New domains take up to ten minutes to initialize\. After your domain is initialized, you can upload data and make changes to the domain\.

**To create an Amazon ES domain \(AWS CLI\)**

+ Run the following command to create an Amazon ES domain\.

  The command creates a domain named *movies* with Elasticsearch version 6\.0\. It specifies one instance of the `t2.small.elasticsearch` instance type\. The instance type requires EBS storage, so it specifies a 10 GB volume\. Finally, the command applies an IP\-based access policy that restricts access to the domain to a single IP address\.

  You need to replace `your_ip_address` in the command with your public IP address, which you can find by searching for "What is my IP?" on [Google](https://www.google.com)\.

  ```
  aws es create-elasticsearch-domain --domain-name movies --elasticsearch-version 6.0 --elasticsearch-cluster-config InstanceType=t2.small.elasticsearch,InstanceCount=1 --ebs-options EBSEnabled=true,VolumeType=standard,VolumeSize=10 --access-policies '{"Version":"2012-10-17","Statement":[{"Effect":"Allow","Principal":{"AWS":"*"},"Action":["es:*"],"Condition":{"IpAddress":{"aws:SourceIp":["your_ip_address"]}}}]}'
  ```

**Note**  
Initializing a domain and its resources takes approximately ten minutes\. When initialization is complete, the endpoint of the domain is available for index and Amazon ES requests\.

Use the following command to query the status of the new domain:

```
aws es describe-elasticsearch-domain --domain movies
```

**To create an Amazon ES domain \(AWS SDKs\)**

The AWS SDKs \(except the Android and iOS SDKs\) support all the actions defined in the Amazon ES Configuration API Reference, including the `CreateElasticsearchDomain` action\. For more information about installing and using the AWS SDKs, see [AWS Software Development Kits](http://aws.amazon.com/code)\. 