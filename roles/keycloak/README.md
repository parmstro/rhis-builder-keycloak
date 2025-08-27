### Role: keycloak

#### Configuration

The primary configuration of the keycloak instance is managed by the main.yml configuration file in the group_vars/keycloak_servers directory.

#### Vaulting Secrets

Like all rhis-builder repos, the secrets are managed by ansible_vault. If you prefer another method, you are free to extend the implementation to support alternatives. 

The expected vaulted secrets are:

- **keycloak_admin_password_vault:** - This is the temporary admin password used to bootstrap the keycloak instance. You will create a permanent keycloak admin after deployment.

- **keycloak_db_user_password_vault:** - This is the password for the keycloak database user. 

- **satellite_username_vault:** - The username for accessing the Satellite API, if registering to Satellite.

- **satellite_password_vault:** - The password for the user accessing the Satellite API, if registering to Satellite.

- **ipa_admin_principal_vault** - The principal allowed to enrole systems to Identity Management, if registering to IdM.

- **ipa_admin_password_vault** - The password for the principal allowed to enrole systems to Identity Management, if registering to IdM.

- **ssl_private_key_passphrase_vault** - The passphrase for the SSL private key to be used when generating certificates for keycloak web service with Identity Management. 

The role uses no_log appropriately to suppress confidential information in logging and debug output.

#### Required variables


#### Optional variables

##### Satellite:
- **register_satellite:** - Set this to true to enable the role to pull and execute the global registration template from Satellite.

- **satellite_username:** - The username of a satellite user able to register hosts. Defaults to the vaulted value.

- **satellite_password:** - The password of a satellite user able to register hosts. Defaults to the vaulted value.
    **OR**
- **satellite_use_gssapi:** - Turn on to use GSSAPI for the authenticating executing user to satellite.
    
- **satellite_url:** - The base url of the satellte server e.g. https://satellite.example.ca
    
- **satellite_organization:** - The organization to which the activation key belongs e.g. "Default Organiztion"

- **satellite_activation_keys:** - The name of the activation key to use to register the host.
    
- **satellite_registration_force:** - Set the force option in the satellite global registration script.

- **satellite_registration_insecure:** - Set to true if the executing system does not yet trust the SSL certificate issued to the Satellite server. This is typical if the system was not deployed by Satellite.

- **satellite_validate_certs:** - Set to false if the executing system does not yet trust the SSL certificate issued to the Satellite server. This is typical if the system was not deployed by Satellite.

##### Identity Management Registration:

- **register_idm:** - set to true to cause role to register the system to identity management.

- **ipaadmin_principal:** - Defaults to the vaulted value.  
- **ipaadmin_password:** - Defaults to the vaulted value.  
- **ipaclient_domain:** - The dns domain containing the IdM servers. 
- **ipaclient_mkhomedir:** - Set to true to have home directories created for IdM users logging into the system.
- **ipaclient_ntp_servers:** - A list of time servers (IP or FQDN)

##### Identity Management Certificates:

- **certificates_idm:** - Set to true to have 

NOTE: There are additional variables defined beyond the ones noted here, however, they are internal and typically are not changed.

- **ipa_server_fqdn:** - The FQDN of the identity management server that will generate the certificates.
- **force_regen:** - Force the role to regenerated the certificates on the current run.
- **ssl_private_key_passphrase:** - Defaults to the vaulted value.
- **ssl_private_key_cipher:** - The private key cipher for the key pair used for the csr
- **ssl_private_key_size:** - The size in bytes to the private key
- **csr_email_address:** - The email address of the certificate contact.
- **csr_organization_name:** -  The organization for the certificate.
- **csr_organization_unit_name:** - The OU for the certificate.
- **csr_country_name:** - The country code for the certificate
- **csr_state_or_province_name:** - The state/province for the certificate.
- **csr_locality_name:** - The city name for the certificate.
- **csr_digest:** - The digest type for the Certificate Signing Request.
