# [WIP - Proposal] Metadata API to get supported bindings and their metadata

Cloud-based serverless offerings like AWS LAmbda and Azure Functions abstract the underlying complexity and integration concerns, enabling seamless interaction with external systems. These solutions build and manage connectors to enable the abstraction. Dapr bindings also addresses the same problem space. 
This proposal intends to enable Dapr to be the integration solution that enables no-code platforms to address integration with external services using Dapr bindings instead of building and managing the connectors.

<TO-DO> : < Get a relevant example from Logic Apps team >


This can be achieved by enabling the following two features in Dapr :
1. OpenAPI compliant GET API for metadata to fetch  
    - DAPR bindings by name, type,cert status
    - Connection attributes for each binding
        - Attribute key, data type , description and example value [ to be shown as hint/tooltip ]
        - Attribute type flag [ Is this attribute mandatory ]
    - Supported Actions
        - Request Schema
        - Response Schema
<Proposed JSON Structure to be embedded>
Note : This information is well-documented for some bindings listed below, however this is not supported to be queried via API. 
 - Apple Push Notification Service
 - Azure blob storage
 - AWS S3

Design Details:
- Add the following methods to Input Binding
    - <REMOVE LATER> Register new endpoint to query for meta data
        - /dapr/dapr/pkg/http/api.go > func (a *api) constructMetadataEndpoints() 
            {
			Methods: []string{fasthttp.MethodGet},
			Route:   "metadata/inputbindings",
			Version: apiVersionV1,
			Handler: a.onGetInputBindingsMetadata,
		},

    - GetConnectionMetadata() 

#Related Proposal : 
- make metadata in DAPR API more portable https://github.com/dapr/dapr/issues/4603
- dapr/sig-api proposal: define a spec for metadata keys in Dapr API https://github.com/dapr/sig-api/issues/4
- Capabilities API for components https://github.com/dapr/dapr/issues/4621


2. Delayed connection to the external service using DAPR binding

    [] 2a. Open API compliant POST API to push configuration data to for the binding. Dapr should be able to connect to the right external service using these configuration attributes.

    [] 2b. Enable delayed connection with the external resource via bindings. Today DAPR intiates the connection with the external resource via bindings during startup. The requirement is to allow the user to push the configuration information via API. Dapr would then connect to the corresponding external service via the appropriate binding and enable the integration between different services.

API specification for getting all the supported bindings :

Metadata to retrieve supported bindings and binding related meta data :


