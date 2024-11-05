# helm chart for helloworld application

## Managing the helm chart using helm commandline

- Creating a helm chart

  - Syntax: `helm create <chart-name>`
  - Ex: `helm create helloworld`

    ```
    Creating helloworld
    ```

- Installing the created helm chart
  - Syntax: `helm install <chart-release-name> <chart-name>`
  - Ex: `helm install helloworld-release helloworld/`
    ```
    NAME: helloworld-release
    LAST DEPLOYED: Tue Nov  5 07:43:07 2024
    NAMESPACE: default
    STATUS: deployed
    REVISION: 1
    NOTES:
    1. Get the application URL by running these commands:
    export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services helloworld-release)
    export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
    echo http://$NODE_IP:$NODE_PORT
    ```
- List the available running helm charts
  - `helm list -a`
    ```
    NAME              	NAMESPACE	REVISION	UPDATED                             	STATUS  	CHART           	APP VERSION
    helloworld-release	default  	1       	2024-11-05 07:43:07.440829 +0530 IST	deployed	helloworld-0.1.0	1.16.0
    ```
- Exposing the helloworld helm cart as NodePort service
  - Update the **values.yaml** file `service.type: NodePort`
- Upgrade the helm chart with the latest changes
  - Syntax: `helm upgrade <chart-release-name> <chart-name>`
  - Ex: `helm upgrade helloworld-release helloworld/`
    ```
    Release "helloworld-release" has been upgraded. Happy Helming!
    NAME: helloworld-release
    LAST DEPLOYED: Tue Nov  5 07:44:47 2024
    NAMESPACE: default
    STATUS: deployed
    REVISION: 2
    NOTES:
        1. Get the application URL by running these commands:
        export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services helloworld-release)
        export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
        echo http://$NODE_IP:$NODE_PORT
    ```
- View the updated revision of helm chart
  - `helm list -a`
    ```
    NAME              	NAMESPACE	REVISION	UPDATED                            	STATUS  	CHART           	APP VERSION
    helloworld-release	default  	2       	2024-11-05 07:44:47.21289 +0530 IST	deployed	helloworld-0.1.0	1.16.0
    ```
- Proxy the helloworld-release service using minikube to access the service

  - `minikube service helloworld-release`

    ```
    |-----------|--------------------|-------------|---------------------------|
    | NAMESPACE |        NAME        | TARGET PORT |            URL            |
    |-----------|--------------------|-------------|---------------------------|
    | default   | helloworld-release | http/80     | http://192.168.49.2:30874 |
    |-----------|--------------------|-------------|---------------------------|
    🏃  Starting tunnel for service helloworld-release.
    |-----------|--------------------|-------------|------------------------|
    | NAMESPACE |        NAME        | TARGET PORT |          URL           |
    |-----------|--------------------|-------------|------------------------|
    | default   | helloworld-release |             | http://127.0.0.1:51368 |
    |-----------|--------------------|-------------|------------------------|
    🎉  Opening service default/helloworld-release in default browser...
    ❗  Because you are using a Docker driver on darwin, the terminal needs to be open to run it.

    ```

- Delete the helm chart
  - Syntax: `helm uninstall <chart-release-name>`
  - Ex: `helm uninstall helloworld-chart`
    ```
    release "helloworld-release" uninstalled
    ```

## Managing the helm chart using helmfile commandline

- Create a helmfile with filename **`helmfile.yaml`** with the following content
  ```
  ---
  releases:
  - name: helloworld-release
    chart: .
    installed: <flag>
  ```
- To install the helmfile we need to update the installed flag vaule with true **`releases[].installed = true`** in helmfile.yaml
- To uninstall the helmfile we need to update the installed flag vaule with false **`releases[].installed = false`** in helmfile.yaml
