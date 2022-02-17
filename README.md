# eventstreaminstall
  
1. Create a project in OpenShift  
  
2. {OpenShift 4.6) Create the CA certificate to a ConfigMap called kube-root-ca.crt in the project (namespace)  
```
oc get cm default-ingress-cert --namespace=openshift-config-managed -o yaml | sed 's/name: default-ingress-cert/name: kube-root-ca.crt/' | sed 's/namespace: openshift-config-managed/namespace: <target-namespace>/' | oc create -f -
```
3. Add IBM Operator Catalog to the OCP Cluster
```
apiVersion: operators.coreos.com/v1alpha1
kind: CatalogSource
metadata:
   name: ibm-operator-catalog
   namespace: openshift-marketplace
spec:
   displayName: "IBM Operator Catalog"
   publisher: IBM
   sourceType: grpc
   image: icr.io/cpopen/ibm-operator-catalog
   updateStrategy:
     registryPoll:
       interval: 45m
```
4. Add IBM Common Services Operator Catalog
```
apiVersion: operators.coreos.com/v1alpha1
kind: CatalogSource
metadata:
   name: opencloud-operators
   namespace: openshift-marketplace
spec:
   displayName: "IBMCS Operators"
   publisher: IBM
   sourceType: grpc
   image: icr.io/cpopen/ibm-common-service-catalog:latest
   updateStrategy:
     registryPoll:
       interval: 45m
```
5. Create image pull secret in the project 
```
oc create secret docker-registry ibm-entitlement-key --docker-username=cp --docker-password="<your-entitlement-key>" --docker-server="cp.icr.io" -n <target-namespace>
```
6. Install Event Streams operator and create an instance
