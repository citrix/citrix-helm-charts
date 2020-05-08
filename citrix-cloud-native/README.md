# Citrix Cloud Native 

[Citrix Cloud Native solutions](https://www.citrix.com/products/citrix-adc/resources/citrix-application-delivery-solutions-for-cloud-native-applications.html) empower establishments to modernize their application delivery as well as application architecture. They leverage the advanced traffic management, observability, and comprehensive security features of Citrix ADCs to ensure enterprise grade reliability and security. This helm chart enables user to do one touch deployment of one or more cloud native products created and managed by Citrix in [Kubernetes](https://kubernetes.io/) or in [OpenShift](https://www.openshift.com) cluster.

This helm chart can be used to:
1. [Deploy Citrix Ingress Controller for Citrix VPX/MPX](./charts/citrix-ingress-controller/README.md)
2. [Deploy Citrix ADC CPX with Citrix Ingress Controller running as sidecar](./charts/citrix-cpx-with-ingress-controller/README.md)
3. [Deploy Citrix Node Controller](./charts/citrix-node-controller/README.md)
4. [Deploy Observability Exporter](./charts/citrix-observability-exporter/README.md)
5. [Deploy Citrix ADC as an Ingress Gateway in Istio environment](./charts/citrix-adc-istio-ingress-gateway)
6. [Deploy Citrix ADC CPX as a sidecar in Istio environment](./charts/citrix-cpx-istio-sidecar-injector)


Depending on the architecture, it is sometimes needed to deploy multiple products in Citrix portfolio together. This can be achieved by setting required parameters for all products together while installing those products using this helm chart.
For example, both Citrix Ingress Controller and Citrix ADC CPX with Citrix Ingress Controller as side car can be deployed using single helm install command as:

  ```
  helm install cic citrix/citrix-cloud-native --set cic.enabled=true,cic.nsIP=<NSIP of Citrix VPX/MPX>,cic.license.accept=yes,cpx.enabled=true,cpx.license.accept=yes
  ```

For upgrading any existing deployment via this helm chart all the parameters that configures the desired state of system needs to be provided in the helm upgrade command.
For example, if Citrix Ingress Controller is already deployed in the cluster using command:

  ```
  helm install cic citrix/citrix-cloud-native --set cic.enabled=true,cic.nsIP=1.1.1.1,cic.loginFileName=nslogin,cic.license.accept=yes,cic.ingressClass[0]=citrix
  ```
then the Citrix Ingress Controller image can be updated in the already existing deployment using command:

  ```
  helm upgrade cic citrix/citrix-cloud-native --set cic.enabled=true,cic.nsIP=1.1.1.1,cic.loginFileName=nslogin,cic.license.accept=yes,cic.ingressClass[0]=citrix,cic.image=<new-image>
  ```
Alternatively, it is recommended to use same [upgrade-values.yaml](../upgrade-values.yaml) and modify the parameters necessary for upgrade. The yaml file can be used for upgrade like:

  ```
  helm upgrade cic citrix/citrix-cloud-native -f upgrade-values.yaml
  ```

> **Important::** 
> Both the charts [Citrix Ingress Controller](./charts/citrix-ingress-controller/) and [Citrix ADC CPX with Citrix Ingress Controller](./charts/citrix-cpx-with-ingress-controller/) contains all the CRDs that are supported by Citrix. The CRDs gets installed by default whenever these charts are deployed. So whenever you are installing both these charts together please make sure you are deploying CRDs only once. You can do this either by setting `cic.crds.install=false` or by setting `cpx.crds.install=false`.
