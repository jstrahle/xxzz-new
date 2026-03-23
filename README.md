0. Varmista, että asennuksesi on rekisteröity: `sudo subscription-manager register`
1. Varmista, että RHEL on uusin versio ja päivitä se jos tarvetta
2. Asenna git ja ansible core: `sudo dnf install git ansible-core`
3. Asenna vaadittavat ansible-collectionit: `ansible-galaxy collection install community.general ansible.posix fedora.linux_system_roles`
4. Editoi inventoryyn haluamasi hostit (satellite ja capsulet) ja varmista, että vault-tiedostossa on Red Hat portaalin tunnukset. Varmista lisäksi, että inventoryssä olevat hostit ovat valideja palvelinnimiä ja ne ovat resolvattavissa (editoi tarvittaessa /etc/hosts sekä capsule-palvelimelta, että satellite palvelimelta)
5. Aja host prepare playbook: `ansible-playbook -i inventory satellite_host_prepare.yml -e @vault_satellite.yml`
6. Aja repository playbook: `ansible-playbook -i inventory satellite_host_repos.yml -e @vault_satellite.yml`
7. Editoi oikeat arvot vars_satellite.yml tiedostoon ja varmista, että manifesti ja muut tiedostot ovat oikeissa paikoissa (vars-tiedoston mukaisesti)
8. Asenna satellite: `ansible-playbook -i inventory satellite_install.yml -e @vault_satellite.yml -e @vars_satellite.yml`
9. Konfiguroi satellite: `ansible-playbook -i inventory satellite_configure.yaml -e @vault_satellite.yml -e @vars_satellite.yml`
10. Aja capsulen repository playbook: `ansible-playbook -i inventory capsule_repos.yaml -e @vars_capsule.yaml -e @vault_satellite.yml`
11. Aja capsulen install playbook: `ansible-playbook -i inventory capsule_install.yaml -e @vars_capsule.yaml -e @vault_satellite.yml`
