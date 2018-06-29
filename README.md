# wcm-io-devops.jenkins-service

This role controls an Jenkins service on Linux servers and waits until
the startup and shutdown is complete. When shutting down the service the
role waits until all Jobs are build complete.

It also provides a handler to only restart the instance as required from
other roles/playbooks.

## Requirements

This role requires Ansible 2.4 or higher. The role requires an Jenkins
service that can be controlled with the Ansible `service` module to be
installed on the target machine.

## Role Variables

Available variables are listed below, along with their default values:

    jenkins_service_name: jenkins

Name of the jenkins service.

    jenkins_service_state: started

Default service state.

    jenkins_service_admin_username: admin

Jenkins admin username.

    jenkins_service_admin_password: admin

Jenkins admin password.

    jenkins_service_port: 8080

HTTP port of the jenkins instance.

    jenkins_service_hostname: localhost

Hostname of the jenkins instance.

    jenkins_service_url_prefix: ""

Url prefix of the jenkins instance, e.g. when running in tomcat.

    jenkins_service_base_url: "http://{{ jenkins_service_hostname }}:{{ jenkins_service_port }}{{ jenkins_service_url_prefix }}"

The base url of the jenkinst instance.

    jenkins_service_stop_check_retries: 300

The number of retries for the jenkins stop check.

    jenkins_service_stop_check_fail: false

Controls the fail behavior when jobs did not finish in the stop period.
Since pipeline jobs currently hangs when jenkins is in shutdown mode we
will not fail and restart, see
https://issues.jenkins-ci.org/browse/JENKINS-34256.

    jenkins_service_start_check_retries: 120

The number of retries for the jenkins start check.

    jenkins_service_check_delay: 1

The delay between the checks if the instance is started up.

    jenkins_service_restart_blocking_enabled: true

Controls if the restart lockmechanism is active.
Since the role may be duplicated through role dependencies this mechanism avoids multiple restart

    jenkins_service_restart_blocking_seconds: 2

Timespan in seconds in which the jenkins restart is blocked.

## Dependencies

This role has no hard dependencies but interacts heavily with the

* [jenkins-plugins](https://github.com/wcm-io-devops/ansible-jenkins-plugins.git)

role(s).

## Example Playbook

Restarts a Jenkins instance

	- hosts: jenkins
	  roles:
	    - { role: wcm-io-devops.jenkins-service,
	        jenkins_service_state: restarted
	      }

## License

Apache 2.0
