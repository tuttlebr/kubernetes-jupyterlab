# K8s Jupyterlab Bringup

A simple configuration for launching a Jupyterlab on Kubernetes.

## Bringup

1. Create a Container Registry Secret. Assuming your registry credentials are local and with Docker.
   ```sh
   kubectl create secret generic regcred \
       --from-file=.dockerconfigjson=$HOME/.docker/config.json  \
       --type=kubernetes.io/dockerconfigjson
   ```
2. Label the secret as part of the lab.
   ```sh
   kubectl label secret regcred app=jupyter-lab
   ```
3. Deploy the Jupyterlab resources.

   ```sh
   kubectl apply -f jupyter-lab.yaml
   ```

4. Monitor pod resources.
   ```sh
   kubectl get all -l "app=jupyter-lab" -o wide
   ```

## Customizations

Within `jupyter-lab.yaml`...

- _Custom Container Image:_ Modify the `Deployment` `image:` value to your desired image.

- _Custom Entrypoint Command:_ Modify the `Deployment` `command:` value, as a list of strings, to your desired launch arguments.

- _Custom Resource Limits:_ Modify the `Deployment` `resources:` values to your desired hardware limits.

- _Custom nodePort:_ Modify the `Service` `nodePort:` value to your available port or comment out to let your cluster decide.

## Teardown

1. Delete the `Deployment` and `Service` resources.

   ```sh
   kubectl delete -f jupyter-lab.yaml
   ```

2. Delete the `Secret` resource. _(Optional)_
   ```sh
   kubectl delete secret regcred
   ```
