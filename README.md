# eventstreaminstall
  
1. Create a project in OpenShift  
  
2. {OpenShift 4.6) Create the CA certificate to a ConfigMap called kube-root-ca.crt in the project (namespace)  
```
ca.crt/' | sed 's/namespace: openshift-config-managed/namespace: <target-namespace>/' | oc create -f -
```
3. 
