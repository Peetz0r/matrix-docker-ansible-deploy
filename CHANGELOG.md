# 2018-08-17

## Disabling some unnecessary Synapse services

The following services are not necessary, so they have been disabled:
- on the federation port (8448): the `client` service
- on the http port (80): the old Angular `webclient` and the `federation` service

Federation runs only on the federation port (8448) now.
The Client APIs run only on the http port (80) now.


# 2018-08-15

## mxisd Identity Server support

The playbook now sets up an [mxisd](https://github.com/kamax-io/mxisd) Identity Server for you by default.
Additional details are available in [Adjusting mxisd Identity Server configuration](docs/configuring-playbook-mxisd.md).


# 2018-08-14

## Email-sending support

The playbook now configures an email-sending service (postfix) by default.
Additional details are available in [Adjusting email-sending settings](docs/configuring-playbook-email.md).

With this, Matrix Synapse is able to send email notifications for missed messages, etc.


# 2018-08-08


## (BC Break) Renaming playbook variables

The following playbook variables were renamed:

- from `matrix_max_upload_size_mb` to `matrix_synapse_max_upload_size_mb`
- from `matrix_max_log_file_size_mb` to `matrix_synapse_max_log_file_size_mb`
- from `matrix_max_log_files_count` to `matrix_synapse_max_log_files_count`
- from `docker_matrix_image` to `matrix_docker_image_synapse`
- from `docker_nginx_image` to `matrix_docker_image_nginx`
- from `docker_riot_image` to `matrix_docker_image_riot`
- from `docker_goofys_image` to `matrix_docker_image_goofys`
- from `docker_coturn_image` to `matrix_docker_image_coturn`

If you're overriding any of them in your `vars.yml` file, you'd need to change to the new names.


## Renaming Ansible playbook tag

The command for executing the whole playbook has changed.
The `setup-main` tag got renamed to `setup-all`.


## Docker container linking

Changed the way the Docker containers are linked together. The ones that need to communicate with others operate in a `matrix` network now and not in the default bridge network.