# HDInsight - OnDemand with Data Lake Store

Azure has taken the HDInsight product a step further with the “On-Demand” offering.  This enables you to effectively upload the configuration for an HDInsight cluster, which will be provisioned when called by an activity and then automatically destroyed once the activity completes.
HDInsight On-Demand Core Configuration
Basic setup information can be found at the following link for how to setup an On-Demand HDInsight cluster that transforms data in blob storage:
https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-create-linux-clusters-adf

However, there are restrictions with using blob storage such as size limits and throughput limits.  This presents a business problem; we want to use HDInsight On Demand to lower costs and improve automation, but also take advantage of Data Lake Store for a massively scalable and optimized storage solution.  For this it was necessary to seek guidance from non-Microsoft HDInsight implementations, to see how they are leveraging the Azure Data Lake Store.
Additional Configuration for Data Lake Store Integration
After much research it was found that adding just a few simple lines of code are all that is required to configure On-Demand HDInsight to communicate directly with Data Lake Store.  Code was deployed using ARM template and so adjustments to the corresponding .json file was as follows, under the cluster typeProperties section:

"coreConfiguration": { 
                "fs.adl.oauth2.access.token.provider.type": "ClientCredential",
                "fs.adl.oauth2.client.id": "[parameters('ADLSServicePrincipalID')]",
                "fs.adl.oauth2.credential": "[parameters('ADLSServicePrincipalKey')]",
                "fs.adl.oauth2.refresh.url": "https://login.microsoftonline.com/[parameters('tenantID')]/oauth2/token"
                }



