# Union: A Helm Chart for Cloudflare Workers

## Overview

Union is a Helm chart designed to facilitate the self-hosting of applications on Cloudflare Workers using a Kubernetes environment. This project provides a streamlined way to deploy Workerd, the JavaScript/Wasm runtime that powers Cloudflare Workers, alongside Cloudflared for tunneling services. By leveraging Helm, Union allows for scalable and efficient deployments, making it a robust solution for developers looking to harness the power of Cloudflare Workers within their own Kubernetes clusters.

## Deployment with Helm

To deploy this project using Helm, ensure you have Helm installed and properly configured to interact with your Kubernetes cluster.

1. **Clone the Repository**

   ```bash
   git clone https://github.com/willswire/union.git
   cd union
   ```

2. **Configure Your Worker Script**

   The worker script can be configured in two ways:

   a. Using the default script in values.yaml:

   ```yaml
   workerd:
     scriptConfig:
       sourceType: "ConfigMap"
       sourceName: "my-release-my-worker-script"
       sourceKey: "worker.js"
   ```

   b. Using your own ConfigMap:

   ```yaml
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: my-worker-script
   data:
     worker.js: |
       addEventListener('fetch', event => {
         event.respondWith(new Response("Hello from my worker!"));
       });
   ```

3. **Install the Helm Chart**

   Use the following command to install the chart, replacing `union` with your desired release name and target namespace:

   ```bash
   helm install union --create-namespace --namespace union ./
   ```

4. **Verify the Deployment**

   You can check the deployment status by running:

   ```bash
   kubectl get deployments -n union
   ```

5. **Accessing the Service**

   If Cloudflared is enabled, to retrieve the hostname for accessing your service, use:

   ```bash
   kubectl logs deployment/union-cloudflared -n union | grep "|  https:" -B 2 -A 1
   ```

## Contribution and License Guidelines

Contributions to this project are welcome and encouraged! To contribute, please follow the standard fork, branch, commit, and pull request workflow. Ensure that your code adheres to any existing style guides and passes all tests.

This project is licensed under the GNU Affero General Public License v3.0. You're free to use, modify, and distribute this software under the terms of this license. Refer to the [LICENSE](LICENSE) file for detailed information.

## Acknowledgements

Union leverages the power of tools developed by Cloudflare.

- [workerd](https://github.com/cloudflare/workerd): The JavaScript/Wasm runtime powering Cloudflare Workers.
- [cloudflared](https://github.com/cloudflare/cloudflared): The Cloudflare daemon for secure and accelerated outbound connections.
