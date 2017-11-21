# Ansible Role: Stackstorm Deployments

[![Build Status](https://travis-ci.org/EncoreTechnologies/ansible-stackstorm.svg?branch=master)](https://travis-ci.org/EncoreTechnologies/ansible-stackstorm)

Deploys Stackstorm packs and Keys on RHEL/CentOS and Debian/Ubuntu servers.

## Requirements

Requires `curl` and `git` to be installed on the server. These should be installed by default with Stackstorm.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    stackstorm_packs_dir: /opt/stackstorm/packs

Default space where packs are deployed to. If your environment has changed the default location this will have to be overridden

    stackstorm_git_dest: /tmp/ansible-git

For nested packs that need to be cloned first then installed. They git repos will cloned to this location then deleted when finished installing the packs.

    stackstorm_config: /etc/st2/st2.conf

Default configuration file for stackstorm. This is needed to add the newly generated key value information to.

    stackstorm_keys_dir: /etc/st2/keys/

Default keys directory where cloudforms recommends that you put your datastore_key. This is not created by default so can easily be overridden to be moved if your environment calls for it.

    stackstorm_datastor_file: datastore_key.json

Default datastore file name that is reccommended by stackstorm. If this is not already created then it can be renamed easily depending on what your envrionment needs.

    stackstorm_configs_dir: /opt/stackstorm/configs

Default configs directory for stackstorm configs to live in.

    stackstorm_keys:
      - name: "test"
        value: "test value"
        encrypted: false

Stackstorm keys array that lists the keys that need to be added. name and value need to be strings as per stackstorm documentation (https://docs.stackstorm.com/datastore.html#storing-secrets), encrypted value can be true or false (default value is false)

    stackstorm_packs:
      - name: "activedirectory"
      - name: "menandmice"
        repo: "https://github.com/StackStorm-Exchange/stackstorm-menandmice"

Stackstorm packs to be deployed. if repo does not exist, will look at stackstorm exchange for pack, otherwise it will install from git.

    stackstorm_packs:
      - name: "test"
        repo: "git@github.com:test/test.git"
        src: "packs/stackstorm-test"
        nested: true
        refresh_virtual: true

If you have a git repository with several stackstorm packs the nested = true flag needs to be added. If that is added the repo will be cloned to the temp directory and packs installed from the local source. You must specifiy the source location from the pack.

    stackstorm_configs:
      - file: test/test_file.yaml
        local: true
        file: test2/test_file2.yaml

Configs to be copied to the proper stackstorm location. These files can either be already on the stackstorm box by using local: true or they can be copied from a remote source by either specifying local: false or not specifying local at all.

## Dependencies

  None, Just a working stackstorm environment

## Example Playbook

    - hosts: stackstorm
      vars:
        stackstorm_keys:
          - name: "test"
            value: "test value"
            encrypted: false
        stackstorm_packs:
          - name: "activedirectory"
      roles:
        - EncoreTechnologies.ansible-stackstorm

## License

MIT (Expat) / BSD

## Author Information

This role was created in 2017 by [Encore Technologies](https://github.com/EncoreTechnologies)
