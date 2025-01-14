# Changelog
All notable changes to this project will be documented in this file.

The file is generated by [Changie](https://github.com/miniscruff/changie).


## 1.3.1 - 2022-03-02
### Fixed
* [#164](https://github.com/vertica/vertica-kubernetes/issues/164) Order the environment variables that were converted from annotations.  Prior to this fix, it was quite easy to get the statefulset controller to go into a repeated rolling upgrade.  The order ensures the statefulset doesn't appear to change between reconcile cycles.
* [#161](https://github.com/vertica/vertica-kubernetes/issues/161) Tolerate slashes being at the end of the communal endpoint url

## 1.3.0 - 2022-02-18
### Added
* [#146](https://github.com/vertica/vertica-kubernetes/issues/146) All annotations in the CR will be converted to environment variables in the containers.
* [#150](https://github.com/vertica/vertica-kubernetes/issues/150) Allow multiple subclusters to share the same Service object
* [#150](https://github.com/vertica/vertica-kubernetes/issues/150) Support for an online upgrade policy
* [#143](https://github.com/vertica/vertica-kubernetes/issues/143) New helm parameters to control the logging level and log path location for the operator pod
* [#81](https://github.com/vertica/vertica-kubernetes/issues/81) Support for RedHat OpenShift 4.8+
### Fixed
* [#151](https://github.com/vertica/vertica-kubernetes/issues/151) Subcluster names with hyphens were prevented from being the default subcluster.  This caused issues when creating the database and removal of subclusters.

## 1.2.0 - 2021-12-21
### Added
* [#87](https://github.com/vertica/vertica-kubernetes/issues/87) Support for Azure Blob Storage (azb://) as a communal endpoint.
* [#87](https://github.com/vertica/vertica-kubernetes/issues/87) Support for Google Cloud Storage (gs://) as a communal endpoint.
* [#87](https://github.com/vertica/vertica-kubernetes/issues/87) Support for HDFS (webhdfs://) as a communal endpoint.
* [#88](https://github.com/vertica/vertica-kubernetes/issues/88) Support for HDFS (swebhdfs://) as a communal endpoint.
* [#89](https://github.com/vertica/vertica-kubernetes/issues/89) Added the ability to specify custom volume mounts for use within the Vertica container.
* [#91](https://github.com/vertica/vertica-kubernetes/issues/91) Support for Kerberos authentication
* [#94](https://github.com/vertica/vertica-kubernetes/issues/94) Ability to specify custom ssh keys
* [#59](https://github.com/vertica/vertica-kubernetes/issues/59) New initPolicy called ScheduleOnly.  Use this policy when you have a vertica cluster running outside of Kubernetes and you want to provision new nodes to run inside Kubernetes.  Most of the automation is disabled when running in this mode.
### Removed
* [#88](https://github.com/vertica/vertica-kubernetes/issues/88) Removed support for Vertica 10.1.1.  The operator only supports Vertica 11.0.0 or higher.
### Fixed
* [#90](https://github.com/vertica/vertica-kubernetes/issues/90) Timing with scale down that can cause corruption in admintools.conf
* [#99](https://github.com/vertica/vertica-kubernetes/issues/99) The RollingUpdate strategy can kick-in after an image change causing pods in the cluster to restart again.
* [#101](https://github.com/vertica/vertica-kubernetes/issues/101) The image change can be marked complete before we finish the restart of the pods.
* [#113](https://github.com/vertica/vertica-kubernetes/issues/113) Restart of a cluster that has nodes in read-only state.  This is needed to run the operator with Vertica version 11.0.2 or newer.


## 1.1.0 - 2021-10-24
### Added
* [#42](https://github.com/vertica/vertica-kubernetes/issues/42) Added the ability to specify custom volumes for use within sidecars.
* [#57](https://github.com/vertica/vertica-kubernetes/issues/57) Added the ability to specify a custom CA file to authenticate s3 communal storage over https.  Previously https was only allowed for AWS.
* [#54](https://github.com/vertica/vertica-kubernetes/issues/54) Added the ability to mount additional certs in the Vertica container.  These certs can be specified through the new '.spec.certSecrets' parameter.
### Changed
* [#39](https://github.com/vertica/vertica-kubernetes/issues/39) Calls to update_vertica are removed.  The operator will modify admintools.conf for install/uninstall now.  This speeds up the time it takes to scale out.
* [#43](https://github.com/vertica/vertica-kubernetes/issues/43) Start the admission controller webhook as part of the operator pod.  This removes the helm chart and container for the webhook.  To order to use the webhook with the namespace scoped operator, the NamespaceDefaultLabelName feature gate must be enabled (on by default in 1.21+) or the namespace must have the label 'kubernetes.io/metadata.name=<nsName>' set.
* [#46](https://github.com/vertica/vertica-kubernetes/issues/46) Relax the dependency that the webhook requires cert-manager.  The default behaviour is to continue to depend on cert-manager.  But we now allow custom certs to be added through new helm chart parameters.
* [#51](https://github.com/vertica/vertica-kubernetes/issues/51) The operator automatically follows the upgrade procedure when the '.spec.image' is changed.  This removes the upgrade-vertica.sh script that previously handled this outside of the operator.
### Fixed
* [#47](https://github.com/vertica/vertica-kubernetes/issues/47) Communal storage on AWS s3.  The timeouts the operator had set were too low preventing a create DB from succeeding.
* [#58](https://github.com/vertica/vertica-kubernetes/issues/58) Increased the memory limit for the operator pod and made it configurable as a helm parameter.
* [#61](https://github.com/vertica/vertica-kubernetes/issues/61) Allow the AWS region to be specified in the CR.

## 1.0.0 - 2021-08-16

### Added
* Kubernetes operator (verticadb-operator) added to manage the lifecycle of a Vertica cluster
* helm chart (verticadb-operator) added to install the operator
* helm chart (verticadb-webhook) added to install the admission controller webhook
* Standalone tool (vdb-gen) that can be used to create a CR from a database for the purpose of migrating it to Kubernetes.

### Removed
* helm chart (vertica) was removed as it was made obsolete by the verticadb-operator

## 0.1.0 - 2021-04-30

### Added
* Helm chart (vertica) for statefulset deployment
