# Keenious Take Home Challenge

## PART 1 - Kubernetes and HELM

This repo creates high-quality helm charts that allows the Keenious services to communicate using a Kubernetes cluster.

## Deployment

This chart installs a various keenious deployments on a k8s cluster using helm.


### Setting up the Chart values in values.yaml

The following table lists the configurable parameters of the charts.

Parameter | Description | Default
--- | --- | ---
`image.pullPolicy` | Image pull policy | `IfNotPresent | Always`
`masterservice.image.repository` | Image repository | `keenioustakehome.azurecr.io/devops/master-search`
`mlinferenceservice.image.repository` | Image repository | `keenioustakehome.azurecr.io/devops/ml-infer`
`mlsearchservice.image.repository` | Image repository | `keenioustakehome.azurecr.io/devops/ml-search`
`keywordservice.image.repository` | Image repository | `keenioustakehome.azurecr.io/devops/keyword`
`textsearchservice.image.repository` | Image repository | `keenioustakehome.azurecr.io/devops/text-search`
`imageCredentials` | Values for pulling images from Azure Contianer Registry | ``
`imageCredentials.registry` | URL of image registry required for authentication | `https://keenioustakehome.azurecr.io`
`imageCredentials.username` | Username of user to authenticate  | `Azure Service principal ID`
`imageCredentials.password` | Password of user to authenticate  | `Azure Service principal password:`
`imageCredentials.email` | User email | `any email`
`service.type` | Node port to expose endpoints | `NodePort`
`service.port` | Service type | `8000`

### Installing the Chart

To install the chart with a namesapce, use

```console
$ helm install keenious keenious-challenge/ --namespace keenious --create-namespace

```

### Verify the helm deployment

```console
$ kubectl get deployments --namespace keenious
```


### Uninstalling the Chart

To uninstall/delete the `keenious` deployment:

```console
$ helm delete keenious --namespace keenious
```

NOTE: This does _not_ delete all resources created by the helm chart. Check for
secrets, the namespace, and ClusterRoles, etc to be remaining.


### Verify the Solution

```console
$ curl 192.168.64.3:32738/docs 
    <!DOCTYPE html>
    <html>
    <head>
    <link type="text/css" rel="stylesheet" href="https://cdn.jsdelivr.net/npm/swagger-ui-dist@3/swagger-ui.css">
    <link rel="shortcut icon" href="https://fastapi.tiangolo.com/img/favicon.png">
    <title>FastAPI - Swagger UI</title>
    </head>
    <body>
    <div id="swagger-ui">
    </div>
    <script src="https://cdn.jsdelivr.net/npm/swagger-ui-dist@3/swagger-ui-bundle.js"></script>
    <!-- `SwaggerUIBundle` is now available on the page -->
    <script>
    const ui = SwaggerUIBundle({
        url: '/openapi.json',
    oauth2RedirectUrl: window.location.origin + '/docs/oauth2-redirect',
        dom_id: '#swagger-ui',
        presets: [
        SwaggerUIBundle.presets.apis,
        SwaggerUIBundle.SwaggerUIStandalonePreset
        ],
        layout: "BaseLayout",
        deepLinking: true,
        showExtensions: true,
        showCommonExtensions: true
    })
    </script>
    </body>
    </html>
    %                                                
```

```
$ curl -X POST -H "Content-Type: application/json" -d '{ "text": "This is a test query", "n_results": 40, "random_seed": 42 }' HOST_IP:<MASTER-NODEIP>/master-service/search
{"2021510395":0.5576037740027965,"1493939421":0.09130499047884139,"2024082073":0.237320640544452,"2017010756":0.20212778092545436,"2019561905":0.6643944618924286,"2034657245":0.597550428679728,"2151165161":0.8546315126845343,"2282206560":0.12188109945404664,"2026122807":0.3510942511862534,"2036462485":0.0935369562101464,"1892627350":0.1991459396461716,"2154816389":0.424940631028215,"2038592739":0.09201382235069021,"2164322495":0.18646604374080647,"560428265":0.5686770482076335,"947917320":0.4623156357607878,"2551968658":0.20031902626383744,"2560773895":0.5059492430783962,"2169625486":0.7506346797531278,"20583405":0.08287972651947638,"2025484036":0.746245910786141,"2173749593":0.6211326689153931,"2148643507":0.28528002598348134,"2018198967":0.16001443985081035,"2560250566":0.9409770472606851,"2027662184":0.28248362599335236,"2555381087":0.12493703099012667,"2170923568":0.12704515342966463,"2021284584":0.7976559376684098,"2024080816":0.5205923118574399,"2164573249":0.747835344009791,"2157872937":0.6566860567907287,"2288994830":0.45395983195134076,"156340658":0.9627089954013972,"1487372514":0.3153334978413632,"2014893994":0.4691773298769605,"2021116159":0.775135858871937,"2147608847":0.5357806993913378,"2029191562":0.8155696515174818,"23861248":0.4940359837366162}%
```
