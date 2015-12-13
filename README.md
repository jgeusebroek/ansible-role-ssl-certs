# Ansible role: ssl-certs

An Ansible Role that installs self-signed or trusted SSL certificates.

## Requirements

OpenSSL needs to be installed when you want to created self-signed certificates.

## Dependencies

None

## Example Playbook

    ---
    - hosts: all
      sudo: yes

      roles:
         - { role: jgeusebroek.ssl-certs, tags: ["ssl-certs"] }

## Example Variables

	# By default only root can access the created certificates.
	# This can be overridden, but you can also override per certificate (recommended)
	ssl_certs_path_owner: "root"
	ssl_certs_path_group: "root"
	ssl_certs_path_mode: "0700"

	ssl_certs_owner: "root"
	ssl_certs_group: "root"
	ssl_certs_mode: "0600"

	# This is where the certificates will be stored,
	# every certificate will be in it's own subdirectory.
	ssl_certs_path: "/etc/ssl_private"

	# Optionally generate a 2048 bit Diffie-Hellman RSA key
	ssl_dhparam_create: True

	# A dictionary of selfsigned certificates to create. 
	# *Note the optional owner and group values.* 
	ssl_self_signed_certs:
	  "localhost":
	    common_name: '{{ ansible_fqdn }}'
	  "domain.localhost":
	    common_name: 'domain.localhost'
	    owner: 'someuser'	# optional
	    group: 'somegroup'	# optional
	    mode: '0644'		# optional

	# A dictionary of trusted certificates to copy.
	# **IMPORTANT**: Don't store certificates/keys in plain text, use [Ansible Vault](http://docs.ansible.com/ansible/playbooks_vault.html) for security.
		ssl_trusted_certs:
	  "somehostname.com":
	    key: |
	      -----BEGIN RSA PRIVATE KEY-----
	      MIIEpAIBAAKCAQEAv2lvWcOxXjBbjILtHCkDe1qTOW1EYp+3BhdXKiLt0OEkg7+U
		   ....
	      oipcOVYh1Y934h/nmPEWWrWwb4Ng+MgynaGngWFmTykxLVb1A6UfnA==
	      -----END RSA PRIVATE KEY-----
	    pem: |
	      -----BEGIN CERTIFICATE-----
	      MIIDVTCCAj2gAwIBAgIJAMTe7AVm4DF/MA0GCSqGSIb3DQEBCwUAMEExCzAJBgNV
		   ....
	      m/4V2mK3+XVuU0THFnr8MubP1bk1qh2MDkFC+RAjLR6Or8O19EqenEA=
	      -----END CERTIFICATE-----
	    owner: 'someuser' 	# optional
	    group: 'somegroup'	# optional
	    mode: '0644' 		# optional

## License

MIT / BSD

## Author Information

This role was created in 2015 by [Jeroen Geusebroek](http://jeroengeusebroek.nl/).