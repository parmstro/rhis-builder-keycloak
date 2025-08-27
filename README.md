## Purpose

The rhis-builder-keycloak repository contains roles used in the Red Hat Infrastructure Standard to deploy and configure keycloak either as a standalone server or as a containerized deployment (containerized is in progress) to broker identity to systems and applications within the RHIS deployment. 

## Running the roles

Yes, you can embed these roles in other plays. You may just want to use some of our utlity plays instead. 

Normally, we provide a main.yml in the root of the project that you can call to deploy and configure the system from a single play. We will add one once we build out more configuration. 

We also provide the standard RHIS helper plays - run_role, run_role_task and run_task - to help you with configuration development, testing and day 2 operations. 

An example is using run_role_task to update the keycloak unit file.

```
ansible-playbook -i inventory \
                 -e "vault_path=/myvault/rhis_builder_vault.yml" \
                 -e "role_name=keycloak" \
                 -e "task_name=ensure_keycloak_service" \
                 --limit=keycloak_servers \
                 --ask-pass \
                 --ask-vault-password \
                 run_role_task.yml
```
We might run this to test new service hardening configurations.

The roles are outlined below.

## Roles
### keycloak

The keycloak role is used to deploy a keycloak instance. It requires an existing RHEL 9 minimal install instance on baremetal, hypervisor or cloud platform. The following activities will be performed by the role:

  - Deployment configuration validation
  - Register to Satellite Server or RHSM (optional)
  - Install prerequisites, including postgresql and db initialization
  - Ensure firewalld configuration
  - Ensure basic postgresql performance tuning
  - Ensure keycloak database creation and configuration
  - Install keycloak service
  - Register to Identity Management (optional)
  - Generate and manage SSL certificates and service in IdM (optional)
  - Bootstrap Keycloak
  - Finalize Keycloak Service

Please see the [README.md](./roles/keycloak/README.md) for the keycloak role for the configuration details.

### keycloak_post

The keycloak_post role is used to configure the keycloak instance and manage day 2 operations using yaml configuration files and a gitOps methodology. The activities that can be performed by the keycloak_post role include:

  - Create realms
  - Create users and groups
  - we are working on feature set.
