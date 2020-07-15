# Ansible role for Apache2 and php-fpm, especially for WP hosting

This is a simple Ansible role for installing Apache2 and php-fpm via ansible-galaxy.

The role should work on new Debian based distributions.

## Installation

Add this to your `requirements.yml`:

```yml
- src: https://github.com/xcdr/ansible-apache2_php-fpm
  name: xcdr.apache2_php-fpm
```

## Example playbook

```yaml
---

- hosts: myhost
  roles: xcdr.apache2_php-fpm

  vars:
    sites:
      - {
          id: 1001,
          name: "com.example",
          server_name: "example.com",
          server_aliases: ["www.example.com", "beta.example.com"],
          server_admin: webmaster@example.com,
          ssl:
            {
              cert: "/etc/letsencrypt/live/example.com/cert.pem",
              key: "/etc/letsencrypt/live/example.com/privkey.pem",
              ca_chain: "/etc/letsencrypt/live/example.com/fullchain.pem",
            },
            wp: {enabled: yes, wordfence: no},
            php: {memory_limit: 128M, post_max_size: 50M, upload_max_filesize: 45M} # optional overwrite per site
        }
    redirects:
      - {
          server_name: "dummy-site.com",
          server_aliases: ["www.dummy-site.com"],
          uri: "https://example.com/dummy-site/",
          ssl:
            {
              cert: "/etc/letsencrypt/live/dummy-site.com/cert.pem",
              key: "/etc/letsencrypt/live/dummy-site.com/privkey.pem",
              ca_chain: "/etc/letsencrypt/live/dummy-site.com/fullchain.pem",
            }
        }
```

## License

MIT
