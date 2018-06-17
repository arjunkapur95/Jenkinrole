# Ansible Role: Jenkins CI

Installs Jenkins CI on RHEL servers.

## Requirements

Requires a valid registration and subscription.

## Pre Steps
Open up subscription-manager,
1.subscription-manager register
2.subscription-manager auto-attach
3.subscription-manager subscribe (following this you need to enter username and password to obtain a valid subscription)

Once you have an active and valid subscription, run the command sudo yum update
## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    jenkins_package_state: present

The state of the `jenkins` package install. By default this role installs Jenkins but will not upgrade Jenkins (when using package-based installs). If you want to always update to the latest version, change this to `latest`.

    jenkins_hostname: localhost

The system hostname; usually `localhost` works fine. This will be used during setup to communicate with the running Jenkins instance via HTTP requests.

    jenkins_home: /var/lib/jenkins

The Jenkins home directory which, amongst others, is being used for storing artifacts, workspaces and plugins. This variable allows you to override the default `/var/lib/jenkins` location.

    jenkins_http_port: 8080

The HTTP port for Jenkins' web interface.

    jenkins_admin_username: admin
    jenkins_admin_password: admin


    jenkins_plugins: []

Jenkins plugins to be installed automatically during provisioning.

    jenkins_plugins_install_dependencies: yes

Whether Jenkins plugins to be installed should also install any plugin dependencies.

    jenkins_plugins_state: present

Use `latest` to ensure all plugins are running the most up-to-date version.

    jenkins_plugin_updates_expiration: 86400

Number of seconds after which a new copy of the update-center.json file is downloaded. Set it to 0 if no cache file should be used.

    jenkins_plugin_timeout: 30

The server connection timeout, in seconds, when installing Jenkins plugins.

    jenkins_version: "1.644"
    jenkins_pkg_url: "http://www.example.com"
    jenkins_url_prefix: ""

Used for setting a URL prefix for your Jenkins installation. The option is added as `--prefix={{ jenkins_url_prefix }}` to the Jenkins initialization `java` invocation, so you can access the installation at a path like `http://www.example.com{{ jenkins_url_prefix }}`. Make sure you start the prefix with a `/` (e.g. `/jenkins`).

    jenkins_connection_delay: 5
    jenkins_connection_retries: 60

Amount of time and number of times to wait when connecting to Jenkins after initial startup, to verify that Jenkins is running. Total time to wait = `delay` * `retries`, so by default this role will wait up to 300 seconds before timing out.

    # For RedHat/CentOS (role default):
    jenkins_repo_url: http://pkg.jenkins-ci.org/redhat/jenkins.repo
    jenkins_repo_key_url: http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key
   

This role will install the latest version of Jenkins by default (using the official repositories as listed above). You can override these variables (use the correct set for your platform) to install the current LTS version instead:

    # For RedHat/CentOS LTS:
    jenkins_repo_url: http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
    jenkins_repo_key_url: http://pkg.jenkins-ci.org/redhat-stable/jenkins-ci.org.key


    jenkins_init_changes:
      - option: "JENKINS_ARGS"
        value: "--prefix={{ jenkins_url_prefix }}"
      - option: "JENKINS_JAVA_OPTIONS"
        value: "{{ jenkins_java_options }}"

Changes made to the Jenkins init script; the default set of changes set the configured URL prefix and add in configured Java options for Jenkins' startup. You can add other option/value pairs if you need to set other options for the Jenkins init fi

## Example Playbook

```yaml
- hosts: jenkins
  vars:
    jenkins_hostname: jenkins.example.com
  roles:
    - role: Javarole
    - role: Jenkinrole
      become: true
```
