# NSX-T Panel App

The NSX-T Panel app is a lightweight application written in modern JS frameworks for reporting various NSX-T statistics and logical config. This was purpose built for one of my customer who wanted an easy/fast way to report the NSX-T T0 SNAT entries and the associated translations to the namespaces/subnets. A native way to do this today via NSX-T is by configuring RBAC with vIDM, and creating read-only access to the developers or SREs to extract this data. The App is meant to simplify this. 

<img width="1417" alt="Screenshot 2019-03-22 17 31 01" src="https://user-images.githubusercontent.com/21146113/54880448-1a800280-4e5e-11e9-848e-4cf1107e04a0.png">

## Quick highlights:
- Express/NodeJS (backend) and React (frontend).
- User authentication (HTTP basicAuth).
- Reporting various NSX-T health stats like CPU, Mem, Disk and cluster status.
- Reporting some logical config (e.g. Fabric Nodes, Logical Routers, T0 SNAT Translations), etc.
- The app is dockerized with all its dependencies and can be deployed on any K8s cluster with the sample YAML config in this repo.
- The config is external (nothing is hardcoded). Passwords are encoded in a K8s Secret.
- The app is stateless and can run via multiple pods for load balancing or high-availability.

## Usage
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
 
**Deployment:**
- Create first a k8s namespace: ```kubectl create namespace nsxtpanel-app```
- Apply the k8s config: ```kubectl apply -f nsxt-panel-app.yaml -n nsxtpanel-app```
 
## Troubleshooting:

I added some tools to help here like net-tools (e.g. traceroute) and cURL. There is also a shell script called tshoot that you can run after you connect to the container as follows:

```kubectl exec -it <pod-name> -n nsxtpanel-app sh```

then:

```sh tshoot.sh```

<img width="556" alt="TSHOOT" src="https://user-images.githubusercontent.com/21146113/54880508-a09c4900-4e5e-11e9-861c-0a3cc31709d5.png">
