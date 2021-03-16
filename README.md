# Step by Step Guide to Install OES on OpenShift
## Download the latest OES Package

Now untar the OES Package
tar -xvf OES-<Version>.tar.gz

### Prequesites for Installation
- Provide user's quay credentials to get access to the images.
- Ensure to have a service account which allows OES Installation process.

### Installation Process
- Navigate to the Directory
```
cd OES-<Version>/charts/oes
```
- Edit the values.yaml update the "imageCredentials"
    ```
    registry: https://quay.io/
    username: <Quay-UserName>  # Quay username
    password: <Quay-Password> # Quay password
    email: emailaddress@domain.com   # email corresponding to quay registry ID
- Enable True for the below if needed
    - CertManager
    - Ingress
    - OpenLDAP
- Incase, if you are using an Ingress which is already existing. Ensure to update the URL's in the below fields, so that all the spinanker and OES configurations will be done during the installation
```
Spinnaker Deck URL configuration; url overwhich spinnaker deck will be accessed
Spinnaker Gate URL configuration; url overwhich spinnaker gate will be accessed
OES-UI url configuration
OES-Gate url configuration
```

- Update the LDAP details in case if using a different LDAP
- Now, start the installation by executing a helm install command
```
cd OES-<Version>/charts/oes/
helm install oes . --namespace <NameSpace> --timeout 20m

E.g. helm install oes . --namespace oes --timeout 20m
```
- Once the installation is completed below list of containers will be up and running.
```
NAME                                        READY   STATUS      RESTARTS   AGE
oes-autopilot-787685b89f-tkkjg              1/1     Running     3          6h1m
oes-dashboard-5d4bbb6fcf-x8f7q              1/1     Running     0          4h13m
oes-db-0                                    1/1     Running     0          5h46m
oes-gate-7b54554c5d-g7qs2                   1/1     Running     0          5h27m
oes-install-using-hal-nkzkj                 0/1     Completed   0          5h46m
oes-minio-b56cd74df-srrhd                   1/1     Running     0          10h
oes-platform-6f55f98c96-72dxd               1/1     Running     0          5h27m
oes-redis-master-0                          1/1     Running     0          10h
oes-sapor-8cf5c9556-d9mpk                   1/1     Running     0          4h15m
oes-spinnaker-halyard-0                     1/1     Running     0          10h
oes-ui-6fc95cdf94-mj8fc                     1/1     Running     0          114m
oes-visibility-889f6fcdb-vcwx7              1/1     Running     4          6h1m
opa-67c945d7b7-p9mjw                        1/1     Running     0          7h5m
spin-clouddriver-caching-6bf67f45b8-wwtcj   1/1     Running     0          5h37m
spin-clouddriver-ro-75f8d6bbbd-58fnn        1/1     Running     0          5h37m
spin-clouddriver-ro-deck-6b499cd544-69nbt   1/1     Running     0          5h37m
spin-clouddriver-rw-6f8ff48976-8c27m        1/1     Running     0          5h37m
spin-deck-7f74cd96d5-wcc5n                  1/1     Running     0          5h37m
spin-echo-scheduler-5fdd9fb7b-m7ztn         1/1     Running     0          5h37m
spin-echo-worker-5bf4b855fd-qzpp8           1/1     Running     0          5h37m
spin-fiat-59578d97f9-8mhvg                  1/1     Running     0          5h37m
spin-front50-58c69476b8-pp9g4               1/1     Running     0          5h37m
spin-gate-6c6cdfc649-7lgvw                  1/1     Running     0          5h37m
spin-igor-85d986c45d-dphn5                  1/1     Running     0          5h37m
spin-orca-7f666f4676-sgbzf                  1/1     Running     0          5h37m
spin-rosco-69fcc9cd7c-ppjnx                 1/1     Running     0          5h37m
```

- In case if you are not using any Ingress, now is time to create routes for the below services

```
oes-ui
oes-gate
spin-deck-lb
spin-gate-lb
```

- Once, you have the routes ready. Update the URL's in the values.yaml in the ingress section and do a 'helm upgrade'. This will update all the overrideurl's in spinnaker and other OES configuration.

```
Spinnaker Deck URL configuration; url overwhich spinnaker deck will be accessed
Spinnaker Gate URL configuration; url overwhich spinnaker gate will be accessed
OES-UI url configuration
OES-Gate url configuration
```
- Command to execute a "helm upgrade"
```
helm upgrade oes . --namespace oes --timeout 20m
```

