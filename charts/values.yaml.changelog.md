# `values.yaml` changelog
All the changes should be listed here so the dev-ops team knows what to do when upgrading Renku.

Please follow this convention when adding a new row
* `<type: NEW|EDIT|DELETE> - *<resource name>*: <details>`

----

## Changes on top of Renku 0.4.2
* EDIT - *notebooks.serverOptions*: added `<resource>.order` to control ui rendering order. The default values defined in notebooks are now different.
* DELETE - *graph.knowledgeGraph.services.renku.url*: no need for this anymore as it's linked to `global.renku.domain` internally. Ref: [refactor KG remove services renku URL](https://github.com/SwissDataScienceCenter/renku/commit/6d4a0e5cf02833d193f86a38cc14762609fcd9c0)
* DELETE - *keycloak.keycloak.extraVolumes.-name:realm-secret* : need to delete `renku-realm.json` in order to work with [remove realm template](https://github.com/SwissDataScienceCenter/renku/commit/825c10e72d185bfaff78af8c7693d06cd745014c) commit
* EDIT - *global.gitlab.postgresPassword*: now split into `value` and `overwriteOnHelmUpgrade`, set the latter to true to generate a password or set value to use an existing one.
* EDIT - *global.gitlab.sudoToken*: now split into `value` and `overwriteOnHelmUpgrade`, set the latter to true to generate a password or set value to use an existing one.
* EDIT - *global.keycloak.postgresPassword*: now split into `value` and `overwriteOnHelmUpgrade`, set the latter to true to generate a password or set value to use an existing one.
* NEW - *global.keycloak.password*: split into `value` and `overwriteOnHelmUpgrade`, set the latter to true to generate a password or set value to use an existing one.
* DELETE - *keycloak.keycloak.password*: the admin password for keycloak is now provided through a k8s secret which is set in the global values section.
* EDIT - *global.jupyterhub.postgresPassword*: now split into `value` and `overwriteOnHelmUpgrade`, set the latter to true to generate a password or set value to use an existing one.
* EDIT - *global.graph.dbEventLog.postgresPassword*: now split into `value` and `overwriteOnHelmUpgrade`, set the latter to true to generate a password or set value to use an existing one.
* EDIT - *global.graph.tokenRepository.postgresPassword*: now split into `value` and `overwriteOnHelmUpgrade`, set the latter to true to generate a password or set value to use an existing one.
* EDIT - *notebooks.jupyterhub.hub.db.url*: remove the password (between ":" and "@" from the URL).
* EDIT - *notebooks.jupyterhub.hub.db.extraEnv*: due to the inability of helm to merge maps, all extra environment variables must be explicitly provided after the introduction of :
```
        extraEnv:
          - name: GITLAB_URL
            value: <your-gitlab-url>
          - name: JUPYTERHUB_SPAWNER_CLASS
            value: spawners.RenkuKubeSpawner
          - name: PGPASSWORD
            valueFrom:
              secretKeyRef:
                name: renku-jupyterhub-postgres
                key: jupyterhub-postgres-password
```