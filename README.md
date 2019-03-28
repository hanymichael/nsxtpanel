# NSX-T Panel App

The NSX-T Panel app is a lightweight application written in modern JS frameworks for reporting various NSX-T statistics and logical config. This was purpose built for one of my customer who wanted an easy/fast way to report the NSX-T T0 SNAT entries and the associated translations to the kubernetes namespaces/subnets. A native way to do this today via NSX-T is by configuring RBAC with vIDM, and creating read-only access accounts for the developers or SREs to extract this data. The App is meant to simplify this workflow.

**Frontend UI**

<img width="1417" alt="Screenshot 2019-03-22 17 31 01" src="https://user-images.githubusercontent.com/21146113/54880448-1a800280-4e5e-11e9-848e-4cf1107e04a0.png">

**Backend API**
![image](https://user-images.githubusercontent.com/21146113/55195076-e17cc080-51c4-11e9-9b45-d7a3e42e39ad.png)

## Quick highlights:

- Express/NodeJS (backend) and React (frontend).
- User authentication (HTTP basicAuth).
- Reporting various NSX-T health stats like CPU, Mem, Disk and cluster status.
- Reporting some logical config (e.g. Fabric Nodes, Logical Routers, T0 SNAT Translations), etc.
- The app is dockerized with all its dependencies and can be deployed on any K8s cluster with the sample YAML config in this repo.
- The config is external (nothing is hardcoded). Passwords are encoded in a K8s Secret.
- The app is stateless and can run via multiple pods for load balancing or high-availability.

## Deployment

You can set the config from within the YAML file itself as follows:

**K8 ConfigMap:**

- NSX-T Manager IP Address
- NSS-T Username
- Tier-0 Router ID
- Update interval
- App Brand (e.g. ACME)

**K8s Secret:**

- NSX-T Manager Password
- App Password

**K8s Deployment:**

- Create first a k8s namespace: `kubectl create namespace nsxtpanel-app`
- Apply the k8s deployment: `kubectl apply -f 01-nsxtpanel-app-deployment.yaml -n nsxtpanel-app`

**Option 1: Expose the app with K8s Service Type: LoadBalancer**

- Create a K8s Service: `kubectl apply -f 02-K8s-Service-Option.yaml -n nsxtpanel-app`

**Option 2: Expose the app with K8s Ingress**

- Create a K8s Ingress: `kubectl apply -f 02-K8s-Ingress-Option.yaml -n nsxtpanel-app`

**OPTIONAL: Create a Network Policy with zero-trust**

- Create a K8s NP: `kubectl apply -f 03-Network-Policy-Option.yaml -n nsxtpanel-app`

## Troubleshooting:

I added some tools to help here like net-tools (e.g. traceroute) and cURL. There is also a shell script called tshoot that you can run after you connect to the container as follows:

`kubectl exec -it <pod-name> -n nsxtpanel-app sh`

then:

`sh tshoot.sh`

<img width="556" alt="TSHOOT" src="https://user-images.githubusercontent.com/21146113/54880508-a09c4900-4e5e-11e9-861c-0a3cc31709d5.png">
