To configure a cubernetes cluster to use a production certificate of letsencrypt
the following steps must be performed on a newly installed AKS cluster with the following requirements:
- addon-http-routing must be checked
- cluster must be newly created
- RBAC should not apply

1. Run the following command to install tiller on the cluster:
        helm init
2. Run the following command to install a certificate manager for leaccrypt production:
        helm install stable/cert-manager --set ingressShim.defaultIssuerName=letsencrypt-prod --set ingressShim.defaultIssuerKind=ClusterIssuer --set rbac.create=false --set serviceAccount.create=false
3. deploy the contents of the file name: CA-install.yaml
        3.1 Start from the console in the folder where the files are located and run the following command:
                        kubectl apply -f CA-install.yaml
4. deploy the contents of the file with the name: Cert-install.yaml
        4.1 adjust the domains to the domain name where the certificate is to be applied
        4.2 Start from the console in the folder where the files are located and run the following command:
                        kubectl apply -f Cert-install.yaml
5. deploy the contents of the file with the name: ingress-controller-config.yaml
        5.1 Start from the console in the folder where the files are located and run the following command:
                        kubectl apply -f ingress-controller-config.yaml.yaml
6. deploy the contents of the file with the name: ingress-install.yaml
        6.1 adjust the service and the host for the specific deployment to which these data relates
        6.2 Start from the console in the folder where the files are located and run the following command:
                        kubectl apply -f ingress-install.yaml
7. Deploy your own .yaml files that contain the service (s) and deployment (s) for IdentityServer4 and the client.

Afther the above steps have been performed you should be able to access your own services using HTTPS and you are able to login using the identityserver
(as part of one of the services you can configure in the provided files), you have your own Kubernetes Cluster where authentication is
provided using IdentityServer4!

Some additional things to keep in mind:
- Create CNAME records to the, by DNS Service created, FQDNs.
- Wait for Let's Encrypt to issue you with the required certificates. This can take a while because CNAME records can take some time
        to propagate over all worldwide DNS servers. This can take up to 4 hours...
- If you have any problems implementing these steps, please let me know.