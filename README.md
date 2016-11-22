# Role nginx-proxy 

This roles installs nginx as a proxy server. The settings are for https use only. So make sure you configure your ssl certificate. The dhparam will be replaced with a new genereated version. The generation takes place during the installation. The role also sets some useful defaults for proxy servers.

The security level is *modern* in terms of the definition from the Mozilla foundation (see their generator for more information https://mozilla.github.io/server-side-tls/ssl-config-generator/). The configuration should fit most systems. I generated the templates using Ubuntu 16.04 though. HSTS is enabled. The configuration gets an A+ grade from ssllabs.com . 

The default website is mapped to / . I created and use the role to abstract and secure the connection to my tomcat server.

## Requirements

An ssl certificate and openssl (for the dhparam). If you're looking for a good role I suggest to take a look at geerlingguy.certbot

## Role Variables

NOTE: variables, that need to be overwritten, are written in UPPERCASE

| Name                  | Default Value | Description                        |
| --------------------- | ------------- | -----------------------------------|
| `NP_SERVER_NAME` | NONE | 'A string containing the names the server (and location) shall listen to. I.e. "example.com www.example.com"' |
| `NP_SSL_CERTIFICATE`| NONE | 'The path to your ssl certificate (fullchain.pem). I.e. /etc/letsencrypt/live/diveperience.de/fullchain.pem' |
| `NP_SSL_CERTIFICATE_KEY` | NONE | 'The path to your ssl certificate private key (privkey.pem). I.e. /etc/letsencrypt/live/diveperience.de/privkey.pem' |
| `NP_PROXY_PASS` | NONE | 'The path to the location. I.e. "http://127.0.0.1:8080/example/"' |
| `NP_REWRITE` | NONE | 'Rewrite rule for the website. I.e. "^/example(.*)$ $1 last"' |
| `np_worker_processes` | {{ ansible_processor_count }} |  'The number of worker processes nginx should use. The default is one per core, determined by the ansible setup.' |
| `np_worker_connections` | 1024 | 'The number of connections for each worker_process (including to backend servers). Keep in mind that a browser opens more than one connection to increase the speed.` |
| `np_keep_alive_timeout` | 15 | 'Timeout for an idle client connection in seconds.' |
| `np_client_max_body_size`| 10m | 'Sets the maximum allowed size of the client request body, specified in the “Content-Length” request header field.' |
| `np_client_body_buffer_size`| 10k | 'Sets buffer size for reading client request body.' |
| `np_proxy_connect_timeout` | 10 | 'Defines a timeout for establishing a connection with a proxied server. It should be noted that this timeout cannot usually exceed 75 seconds.' |
| `np_proxy_send_timeout` | 10 | 'Sets a timeout (in seconds) for transmitting a request to the proxied server.' |
| `np_proxy_read_timeout` | 10 | 'Defines a timeout (in seconds) for reading a response from the proxied server.' |
| `np_proxy_buffers` | "32 8k" | 'Sets the size of the buffer used for reading the first part of the response received from the proxied server. This part usually contains a small response header. By default, the buffer size is equal to one memory page. This is either 4K or 8K, depending on a platform. It can be made smaller, however.' |

Note: parts of this documentation are from the official nginx documentation (https://nginx.org/en/docs/) The sources and documentation are distributed under the 2-clause BSD-like license.

Modify the variables according to your needs.
 
## Dependencies

## Example Playbook

    - hosts: servers
      roles:
         - { role: hoall.nginx-proxy, NP_SERVER_NAME: "example.com www.example.com", NP_SSL_CERTIFICATE: /etc/letsencrypt/live/fullchain.pem, NP_SSL_CERTIFICATE_KEY: /etc/letsencrypt/live/privkey.pem }

## License

BSD

## Author Information

Felix Paetow fhmpaetow@fsfe.org

