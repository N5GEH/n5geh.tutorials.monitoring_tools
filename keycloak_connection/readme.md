# Creating a Client in Keycloak

Changes in the Keycloak admin interface - for detail information about the requirements of oauth2 see: https://oauth2-proxy.github.io/oauth2-proxy/configuration/providers/keycloak_oidc
1) Navigate to Clients in the desired realm
2) Create a new client
    - General Settings
        - `Client type`: OpenID Connect
        - Set `Client ID` uniquely
        - `Name` and `Description` optional
    - Capability Config
        - <img src="./oauth_client_capability_config.png" alt="Capability Config Setup" width="500" />
    - Login Settings
        - `Valid redirect URIs` corresponds to the URL of the desired service
3) Adjustment of the scopes necessary
    - Navigate to the created client / Client Scopes
    - Select the created scope `${Client ID}-dedicated`
    - Add a mapper ("By Configuration"): `Audience` / for Keycloak OIDC needed
        - Name = `aud-mapper-<your client's id>`
        - Included Client Audience = `<your client's id>`
        - Add to ID token and dd to access toke = `ON`
    - Add a mapper ("By Configuration")
        - <img src="./oauth_client_scropes_groups.png" alt="Mapper Group Membership" width="500" />
4) Create the desired group and add the relevant people
5) Transfer the access data to [oauth2-proxy](https://github.com/oauth2-proxy/oauth2-proxy) from the client config
    - <img src="./oauth_client_credentials.png" alt="Client Credentials" width="500" />