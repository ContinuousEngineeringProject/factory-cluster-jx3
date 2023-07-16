# Factory cluster GitOps Repository for GKE and Google Secret Manager

This is the Factory cluster GitOps repository for Jenkins X 3.x with GKE and Google Secret Manager.

## Create a new factory cluster

### Using Kubernetes version 1.23+
1. Remove health-chek-jx chart in helmfiles/jx/helmfile.yaml
    ```yaml
    -  chart: jxgh/jx-kh-check
       version: 0.0.78
       condition: jxRequirementsKuberhealthy.enabled
       name: health-checks-jx
       values:
       - ../../versionStream/charts/jxgh/health-checks-jx/values.yaml.gotmpl
       - jx-values.yaml
    ```
2. In the same file (helmfile.yaml) include in the acme-jx chart 2 statements to disable issuer.
    ```yaml
    - chart: jxgh/acme
      version: 0.0.24
      condition: jxRequirementsIngressTLS.enabled
      name: acme-jx
      values:
      - ../../versionStream/charts/jxgh/acme-jx/values.yaml.gotmpl
      - jx-values.yaml
      - issuer:             ## Include this line
          enabled: false    ## Inlude this line
    ```
3. (Optional) To set your staging environment URLs to be different from your non-staging urls modify the jx-requirements.yml ‘environments:’ and ‘ingress:’ sections with the following:
    ```yaml
    environments:
    - key: dev
    - key: staging
      ingress:
         namespaceSubDomain: -stg.
    - key: production
    ingress:
      domain: ""
      externalDNS: false
      namespaceSubDomain: .
      tls:
        email: ""
        enabled: false
        production: false
    ```
4. Commit and push the changes to the **Factory Infrastructure** git repository.

## Upgrading factory resources

1. Modify the `jx-requirements.yml` file
2. Commit and push the changes to the **Factory Infrastructure** git repository.

