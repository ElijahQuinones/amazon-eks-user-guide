# Review release notes for Kubernetes versions on standard support<a name="kubernetes-versions-standard"></a>

This topic gives important changes to be aware of for each Kubernetes version in standard support\. When upgrading, carefully review the changes that have occurred between the old and new versions for your cluster\.

**Note**  
For `1.24` and later clusters, officially published Amazon EKS AMIs include `containerd` as the only runtime\. Kubernetes versions earlier than `1.24` use Docker as the default runtime\. These versions have a bootstrap flag option that you can use to test out your workloads on any supported cluster with `containerd`\. For more information, see [Amazon EKS ended support for `Dockershim`](dockershim-deprecation.md)\.

## Kubernetes 1\.30<a name="kubernetes-1.30"></a>

Kubernetes `1.30` is now available in Amazon EKS\. For more information about Kubernetes `1.30`, see the [official release announcement](https://kubernetes.io/blog/2024/04/17/kubernetes-v1-30-release/)\.

**Important**  
Starting with Amazon EKS version `1.30` or newer, any newly created managed node groups will automatically default to using Amazon Linux 2023 \(AL2023\) as the node operating system\. Previously, new node groups would default to Amazon Linux 2 \(AL2\)\. You can continue to use AL2 by choosing it as the AMI type when creating a new node group\.   
For more information about Amazon Linux, see [Comparing AL2 and AL2023](https://docs.aws.amazon.com/linux/al2023/ug/compare-with-al2.html) in the Amazon Linux User Guide\. 
For more information about specifiying the operating system for a managed node group, see [Creating a managed node group](create-managed-node-group.md)\.
+ With Amazon EKS `1.30`, the `topology.k8s.aws/zone-id` label is added to worker nodes\. You can use Availability Zone IDs \(AZ IDs\) to determine the location of resources in one account relative to the resources in another account\. For more information, see [Availability Zone IDs for your AWS resources](https://docs.aws.amazon.com/ram/latest/userguide/working-with-az-ids.html) in the *AWS RAM User Guide*\. 
+ Starting with `1.30`, Amazon EKS no longer includes the `default` annotation on the `gp2` `StorageClass` resource applied to newly created clusters\. This has no impact if you are referencing this storage class by name\. You must take action if you were relying on having a default `StorageClass` in the cluster\. You should reference the `StorageClass` by the name `gp2`\. Alternatively, you can deploy the Amazon EBS recommended default storage class by setting the `defaultStorageClass.enabled` parameter to true when installing `v1.31.0` or later of the `aws-ebs-csi-driver add-on`\. 
+ The minimum required IAM policy for the Amazon EKS cluster IAM role has changed\. The action `ec2:DescribeAvailabilityZones` is required\. For more information, see [Amazon EKS cluster IAM role](service_IAM_role.md)\.

For the complete Kubernetes `1.30` changelog, see [https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.30.md](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.30.md)\.

## Kubernetes 1\.29<a name="kubernetes-1.29"></a>

Kubernetes `1.29` is now available in Amazon EKS\. For more information about Kubernetes `1.29`, see the [official release announcement](https://kubernetes.io/blog/2023/12/13/kubernetes-v1-29-release/)\.

**Important**  
The deprecated `flowcontrol.apiserver.k8s.io/v1beta2` API version of `FlowSchema` and `PriorityLevelConfiguration` are no longer served in Kubernetes `v1.29`\. If you have manifests or client software that uses the deprecated beta API group, you should change these before you upgrade to `v1.29`\.
+ The `.status.kubeProxyVersion` field for node objects is now deprecated, and the Kubernetes project is proposing to remove that field in a future release\. The deprecated field is not accurate and has historically been managed by `kubelet` \- which does not actually know the `kube-proxy` version, or even whether `kube-proxy` is running\. If you've been using this field in client software, stop \- the information isn't reliable and the field is now deprecated\.
+ In Kubernetes `1.29` to reduce potential attack surface, the `LegacyServiceAccountTokenCleanUp` feature labels legacy auto\-generated secret\-based tokens as invalid if they have not been used for a long time \(1 year by default\), and automatically removes them if use is not attempted for a long time after being marked as invalid \(1 additional year by default\)\. To identify such tokens, a you can run: 

  ```
  kubectl get cm kube-apiserver-legacy-service-account-token-tracking -n kube-system
  ```

For the complete Kubernetes `1.29` changelog, see [https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.29.md#changelog-since-v1280](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.29.md#changelog-since-v1280)\.

## Kubernetes 1\.28<a name="kubernetes-1.28"></a>

Kubernetes `1.28` is now available in Amazon EKS\. For more information about Kubernetes `1.28`, see the [official release announcement](https://kubernetes.io/blog/2023/08/15/kubernetes-v1-28-release/)\.
+ Kubernetes `v1.28` expanded the supported skew between core node and control plane components by one minor version, from `n-2 `to `n-3`, so that node components \(`kubelet` and `kube-proxy`\) for the oldest supported minor version can work with control plane components \(`kube-apiserver`, `kube-scheduler`, `kube-controller-manager`, `cloud-controller-manager`\) for the newest supported minor version\.
+ Metrics `force_delete_pods_total` and `force_delete_pod_errors_total` in the `Pod GC Controller` are enhanced to account for all forceful pods deletion\. A reason is added to the metric to indicate whether the pod is forcefully deleted because it's terminated, orphaned, terminating with the out\-of\-service taint, or terminating and unscheduled\.
+ The `PersistentVolume (PV)` controller has been modified to automatically assign a default `StorageClass` to any unbound `PersistentVolumeClaim` with the `storageClassName` not set\. Additionally, the `PersistentVolumeClaim` admission validation mechanism within the API server has been adjusted to allow changing values from an unset state to an actual `StorageClass `name\.

For the complete Kubernetes `1.28` changelog, see [https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.28.md#changelog-since-v1270](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.28.md#changelog-since-v1270)\.

## Kubernetes 1\.27<a name="kubernetes-1.27"></a>

Kubernetes `1.27` is now available in Amazon EKS\. For more information about Kubernetes `1.27`, see the [official release announcement](https://kubernetes.io/blog/2023/04/11/kubernetes-v1-27-release/)\.

**Important**  
The support for the alpha `seccomp` annotations `seccomp.security.alpha.kubernetes.io/pod` and `container.seccomp.security.alpha.kubernetes.io` annotations was removed\. The alpha `seccomp` annotations was deprecated in `1.19`, and with their removal in `1.27`, `seccomp` fields will no longer auto\-populate for `Pods` with `seccomp` annotations\. Instead, use the `securityContext.seccompProfile` field for `Pods` or containers to configure `seccomp` profiles\. To check whether you are using the deprecated alpha `seccomp` annotations in your cluster, run the following command:  

  ```
  kubectl get pods --all-namespaces -o json | grep -E 'seccomp.security.alpha.kubernetes.io/pod|container.seccomp.security.alpha.kubernetes.io'
  ```
The `--container-runtime` command line argument for the `kubelet` was removed\. The default container runtime for Amazon EKS has been `containerd` since `1.24`, which eliminates the need to specify the container runtime\. From `1.27` onwards, Amazon EKS will ignore the `--container-runtime` argument passed to any bootstrap scripts\. It is important that you don't pass this argument to `--kubelet-extra-args` in order to prevent errors during the node bootstrap process\. You must remove the `--container-runtime` argument from all of your node creation workflows and build scripts\.
+ The `kubelet` in Kubernetes `1.27` increased the default `kubeAPIQPS` to `50` and `kubeAPIBurst` to `100`\. These enhancements allow the `kubelet` to handle a higher volume of API queries, improving response times and performance\. When the demands for `Pods` increase, due to scaling requirements, the revised defaults ensure that the `kubelet` can efficiently manage the increased workload\. As a result, `Pod` launches are quicker and cluster operations are more effective\.
+ You can use more fine grained `Pod` topology to spread policies such as `minDomain`\. This parameter gives you the ability to specify the minimum number of domains your `Pods` should be spread across\. `nodeAffinityPolicy` and `nodeTaintPolicy` allow for an extra level of granularity in governing `Pod` distribution\. This is in accordance to node affinities, taints, and the `matchLabelKeys` field in the `topologySpreadConstraints` of your `Pod's` specification\. This permits the selection of `Pods` for spreading calculations following a rolling upgrade\.
+ Kubernetes`1.27` promoted to beta a new policy mechanism for `StatefulSets` that controls the lifetime of their `PersistentVolumeClaims`\(`PVCs`\)\. The new `PVC` retention policy lets you specify if the `PVCs` generated from the `StatefulSet` spec template will be automatically deleted or retained when the `StatefulSet` is deleted or replicas in the `StatefulSet` are scaled down\.
+ The [https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/) option in the Kubernetes API server helps prevent `HTTP/2` client connections from being stuck on a single API server instance, by randomly closing a connection\. When the connection is closed, the client will try to reconnect, and will likely land on a different API server as a result of load balancing\. Amazon EKS version `1.27` has enabled `goaway-chance` flag\. If your workload running on Amazon EKS cluster uses a client that is not compatible with [https://www.rfc-editor.org/rfc/rfc7540#section-6.8](https://www.rfc-editor.org/rfc/rfc7540#section-6.8), we recommend that you update your client to handle `GOAWAY` by reconnecting on connection termination\.

For the complete Kubernetes `1.27` changelog, see [https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.27.md#changelog-since-v1260](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.27.md#changelog-since-v1260)\.