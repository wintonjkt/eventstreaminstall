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

## Demo Event Streams using IBM Cloud Public
  
  1. Create a topic with name "kafka-java-console-sample-topic"  
  2. Create credentials
     Go to Service credentials in the navigation pane --> New credential --> Enter name, Manager role, Add. 
     Get the api_key and kafka_brokers_sasl values
  3. Clone the demo git repo
```
git clone https://github.com/ibm-messaging/event-streams-samples.git
```
  4. Build the sample app using Gradle
```
cd event-streams-samples/kafka-java-console-sample
gradle clean && gradle build
```
  5. Run the sample app
```ava -jar ./build/libs/kafka-java-console-sample-2.0.jar <kafka_brokers_sasl> <api_key> -consumerple-2.0.jar 
```
  Use the kafka_brokers_sasl from the Service credentials created in Step 2. Use all the kafka_brokers_sasl listed in the Service credentials that you created.
  The kafka_brokers_sasl must be formatted as "host:port,host2:port2".
  Format the contents of kafka_brokers_sasl in a text editor before you enter it in the command line.
  Then, use the api_key from the Service credentials created in Step 2. -consumer indicates to start the consumer.

  6. Run the producing application.

```
cd event-streams-samples/kafka-java-console-sample
java -jar ./build/libs/kafka-java-console-sample-2.0.jar <kafka_brokers_sasl> <api_key> -producer
```




