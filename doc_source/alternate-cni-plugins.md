# Alternate CNI plugins for Amazon EKS clusters<a name="alternate-cni-plugins"></a>

The [https://github.com/aws/amazon-vpc-cni-plugins](https://github.com/aws/amazon-vpc-cni-plugins) is the only CNI plugin supported by Amazon EKS\. Amazon EKS runs upstream Kubernetes, so you can install alternate compatible CNI plugins to Amazon EC2 nodes in your cluster\. If you have Fargate nodes in your cluster, the Amazon VPC CNI plugin for Kubernetes is already on your Fargate nodes\. It's the only CNI plugin you can use with Fargate nodes\. An attempt to install an alternate CNI plugin on Fargate nodes fails\.

If you plan to use an alternate CNI plugin on Amazon EC2 nodes, we recommend that you obtain commercial support for the plugin or have the in\-house expertise to troubleshoot and contribute fixes to the CNI plugin project\. 

Amazon EKS maintains relationships with a network of partners that offer support for alternate compatible CNI plugins\. For details about the versions, qualifications, and testing performed, see the following partner documentation\.


| Partner | Product | Documentation | 
| --- | --- | --- | 
| Tigera | [Calico](https://www.tigera.io/partners/aws/) | [Installation instructions](https://docs.projectcalico.org/getting-started/kubernetes/managed-public-cloud/eks) | 
| Isovalent | [Cilium](https://cilium.io) | [Installation instructions](https://docs.cilium.io/en/stable/gettingstarted/k8s-install-default/) | 
| Juniper | [Cloud\-Native Contrail Networking \(CN2\)](https://www.juniper.net/us/en/products/sdn-and-orchestration/contrail/cloud-native-contrail-networking.html) | [Installation instructions](https://www.juniper.net/documentation/us/en/software/cn-cloud-native23.2/cn-cloud-native-eks-install-and-lcm/index.html) | 
| VMware | [Antrea](https://antrea.io/) | [Installation instructions](https://antrea.io/docs/main/docs/eks-installation) | 

Amazon EKS aims to give you a wide selection of options to cover all use cases\.

## Alternate compatible network policy plugins<a name="alternate-network-policy-plugins"></a>

[Calico](https://www.tigera.io/project-calico) is a widely adopted solution for container networking and security\. Using Calico on EKS provides a fully compliant network policy enforcement for your EKS clusters\. Additionally, you can opt to use Calico's networking, which conserve IP addresses from your underlying VPC\. [Calico Cloud](https://www.tigera.io/tigera-products/calico-cloud/) enhances the features of Calico Open Source, providing advanced security and observability capabilities\.

Traffic flow to and from Pods with associated security groups are not subjected to Calico network policy enforcement and are limited to Amazon VPC security group enforcement only\. 

If you use Calico network policy enforcement, we recommend that you set the environment variable `ANNOTATE_POD_IP` to `true` to avoid a known issue with Kubernetes\. To use this feature, you must add `patch` permission for pods to the `aws-node` ClusterRole\. Note that adding patch permissions to the `aws-node` DaemonSet increases the security scope for the plugin\. For more information, see [ANNOTATE\_POD\_IP](https://github.com/aws/amazon-vpc-cni-k8s/?tab=readme-ov-file#annotate_pod_ip-v193) in the VPC CNI repo on GitHub\.