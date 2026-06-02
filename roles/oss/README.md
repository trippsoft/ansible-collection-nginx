<!-- BEGIN_ANSIBLE_DOCS -->

# Ansible Role: trippsc2.nginx.oss
Version: 1.3.0

This role configures NGINX OSS on Linux machines.

## Requirements

| Platform | Versions |
| -------- | -------- |
| Debian | <ul><li>trixie</li></ul> |
| EL | <ul><li>10</li><li>9</li><li>8</li></ul> |
| Ubuntu | <ul><li>resolute</li><li>noble</li><li>jammy</li></ul> |

## Dependencies

| Collection |
| ---------- |
| ansible.posix |
| ansible.utils |
| community.general |

## Role Arguments
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| nginx_appstream_version | <p>The version of NGINX to install via AppStream packages on EL 8 and 9 systems.</p><p>On other systems, this is ignored.</p><p>See https://access.redhat.com/support/policy/updates/rhel-app-streams-life-cycle for the list of available versions for each EL major version.</p> | str | no |  | 1.24 |
| nginx_configure_selinux | <p>Whether to configure SELinux.</p><p>Packages required to managed SELinux are installed if this is set to `true`.</p><p>For Debian-based systems, this will default to `false`.</p><p>For EL systems, this will default to `true`.</p> | bool | no |  |  |
| nginx_configure_firewall | <p>Whether to configure a host firewall software for use with NGINX.</p> | bool | no |  | True |
| nginx_configure_logrotate | <p>Whether to configure logrotate to manage nginx log files.</p> | bool | no |  | True |
| nginx_logrotate_paths | <p>The list of paths to log files to rotate.</p><p>If *nginx_configure_logrotate* is set to `false`, this is ignored.</p> | list of 'path' | no |  | ['/var/log/nginx/*.log'] |
| nginx_logrotate_period | <p>The period at which logrotate will rotate log files.</p><p>If *nginx_configure_logrotate* is set to `false`, this is ignored.</p> | str | no | <ul><li>daily</li><li>weekly</li><li>monthly</li></ul> | daily |
| nginx_logrotate_retention | <p>The number of days to retain logs.</p><p>If *nginx_configure_logrotate* is set to `false`, this is ignored.</p> | int | no |  | 14 |
| nginx_logrotate_mode | <p>The mode of the log files.</p> | str | no |  | 0640 |
| nginx_user | <p>The user as which to run NGINX.</p><p>For Debian-based systems, this will default to `www-data`.</p><p>For EL systems, this will default to `nginx`.</p><p>If overridden from the default user, the user will **not** be created and must already exist.</p> | str | no |  |  |
| nginx_group | <p>The group as which to run NGINX.</p><p>For Debian-based systems, this will default to `www-data`.</p><p>For EL systems, this will default to `nginx`.</p><p>If overridden from the default group, the group will **not** be created and must already exist.</p> | str | no |  |  |
| nginx_firewall_type | <p>The type of host firewall software to configure for use with NGINX.</p><p>For EL or Debian systems, this will default to `firewalld`.</p><p>For Ubuntu systems, this will default to `ufw`.</p> | str | no | <ul><li>firewalld</li><li>ufw</li></ul> |  |
| nginx_allow_http | <p>Whether to allow HTTP (TCP port 80) traffic in firewalld or ufw.</p> | bool | no |  | True |
| nginx_allow_https | <p>Whether to allow HTTPS (TCP port 443) traffic in firewalld or ufw.</p> | bool | no |  | False |
| nginx_tcp_ports | <p>The list of TCP ports on which NGINX will be listening.</p><p>This list is in addition to ports 80 and 443 which are controlled by *nginx_allow_http* and *nginx_allow_https*.</p><p>If *nginx_configure_firewall* is set to `true`, these ports will be allowed in the firewalld or ufw.</p><p>If *nginx_configure_selinux* is set to `true`, these ports will be classified as `http_port_t`.</p> | list of 'int' | no |  | [] |
| nginx_udp_ports | <p>The list of UDP ports on which NGINX will be listening.</p><p>If *nginx_configure_firewall* is set to `true`, these ports will be allowed in the firewalld or ufw.</p><p>If *nginx_configure_selinux* is set to `true`, these ports will be classified as `http_port_t`.</p> | list of 'int' | no |  | [] |
| nginx_worker_cpu_affinity | <p>The CPU affinity configuration for NGINX worker processes.</p><p>https://nginx.org/en/docs/ngx_core_module.html#worker_cpu_affinity</p> | dict of 'nginx_worker_cpu_affinity' options | no |  |  |
| nginx_worker_priority | <p>The scheduling priority for worker processes.</p><p>Must be between -20 and 20.</p><p>https://nginx.org/en/docs/ngx_core_module.html#worker_priority</p> | int | no |  |  |
| nginx_worker_processes | <p>The worker processes configuration.</p><p>https://nginx.org/en/docs/ngx_core_module.html#worker_processes</p> | dict of 'nginx_worker_processes' options | no |  | {'auto': True} |
| nginx_worker_rlimit_core | <p>The core file size limit for worker processes.</p><p>https://nginx.org/en/docs/ngx_core_module.html#worker_rlimit_core</p> | str | no |  |  |
| nginx_worker_rlimit_nofile | <p>The maximum number of open files for worker processes.</p><p>https://nginx.org/en/docs/ngx_core_module.html#worker_rlimit_nofile</p> | int | no |  |  |
| nginx_worker_shutdown_timeout | <p>The graceful shutdown timeout for worker processes.</p><p>https://nginx.org/en/docs/ngx_core_module.html#worker_shutdown_timeout</p> | str | no |  |  |
| nginx_error_log | <p>The error log configuration.</p><p>https://nginx.org/en/docs/ngx_core_module.html#error_log</p> | dict of 'nginx_error_log' options | no |  | {'file': '/var/log/nginx/error.log', 'level': 'info'} |
| nginx_pid | <p>The path to the PID file.</p><p>https://nginx.org/en/docs/ngx_core_module.html#pid</p> | path | no |  | /var/run/nginx.pid |
| nginx_env | <p>Environment variables to set for NGINX.</p><p>The name of the environment variable is the key, and the value is the value.</p><p>https://nginx.org/en/docs/ngx_core_module.html#env</p> | dict | no |  |  |
| nginx_pcre_jit | <p>Whether to enable PCRE JIT compilation.</p><p>https://nginx.org/en/docs/ngx_core_module.html#pcre_jit</p> | bool | no |  |  |
| nginx_ssl_engine | <p>The path to SSL acceleration engine to use.</p><p>https://nginx.org/en/docs/ngx_core_module.html#ssl_engine</p> | path | no |  |  |
| nginx_timer_resolution | <p>The interval between attempts to get the system time by system call.</p><p>https://nginx.org/en/docs/ngx_core_module.html#timer_resolution</p> | str | no |  |  |
| nginx_working_directory | <p>The working directory for NGINX.</p><p>https://nginx.org/en/docs/ngx_core_module.html#working_directory</p> | path | no |  |  |
| nginx_events_multi_accept | <p>Whether to accept multiple connections with one system call.</p><p>https://nginx.org/en/docs/ngx_core_module.html#multi_accept</p> | bool | no |  |  |
| nginx_events_worker_aio_requests | <p>The maximum number of asynchronous I/O operations for a single worker process.</p><p>https://nginx.org/en/docs/ngx_core_module.html#worker_aio_requests</p> | int | no |  |  |
| nginx_events_worker_connections | <p>The maximum number of connections for a single worker process.</p><p>https://nginx.org/en/docs/ngx_core_module.html#worker_connections</p> | int | no |  | 1024 |
| nginx_http_includes | <p>The list of HTTP include files.</p><p>https://nginx.org/en/docs/ngx_core_module.html#include</p> | list of 'path' | no |  | ['/etc/nginx/mime.types', '/etc/nginx/conf.d/*.conf'] |
| nginx_http_config_files | <p>The list of HTTP configuration files.</p> | list of dicts of 'nginx_http_config_files' options | no |  | [{'destination': '/etc/nginx/conf.d/default.conf', 'client_body_timeout': 10, 'client_header_timeout': 10, 'keepalive_timeout': {'timeout': 10}, 'send_timeout': 10, 'large_client_header_buffers': {'count': 2, 'size': '1k'}, 'log_format': [{'name': 'main', 'format': '\'server="$server_name" host="$host” dest_port="$server_port" \'\n\'src="$remote_addr" ip="$realip_remote_addr" user="$remote_user" \'\n\'time_local="$time_local" http_status="$status" \'\n\'http_referer="$http_referer" http_user_agent="$http_user_agent" \'\n\'http_x_forwarded_for="$http_x_forwarded_for" \'\n\'http_x_header="$http_x_header" uri_query="$query_string" uri_path="$uri" \'\n\'request=$request http_method="$request_method"\''}], 'server': [{'listen': [{'port': 80}], 'server_name': '_', 'error_page': [{'codes': [500, 502, 503, 504], 'uri': '/50x.html'}], 'access_log': {'logs': [{'path': '/var/log/nginx/access.log', 'format': 'main'}]}, 'location': [{'path': '/', 'root': '/usr/share/nginx/html'}]}]}] |

### Options for nginx_worker_cpu_affinity
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| auto | <p>Whether to use automatic CPU affinity.</p><p>https://nginx.org/en/docs/ngx_core_module.html#worker_cpu_affinity</p> | bool | no |  | False |
| mask | <p>The CPU core affinity mask on which NGINX worker processes will run.</p><p>If *auto* is set to `false`, this is required.</p><p>https://nginx.org/en/docs/ngx_core_module.html#worker_cpu_affinity</p> | str | no |  |  |

### Options for nginx_worker_processes
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| auto | <p>Whether to automatically determine the number of worker processes.</p><p>https://nginx.org/en/docs/ngx_core_module.html#worker_processes</p> | bool | no |  | False |
| processes | <p>The number of worker processes.</p><p>If *auto* is set to `true`, this is ignored.</p><p>Otherwise, this is required.</p><p>https://nginx.org/en/docs/ngx_core_module.html#worker_processes</p> | int | no |  |  |

### Options for nginx_error_log
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| file | <p>The path to the error log file.</p><p>https://nginx.org/en/docs/ngx_core_module.html#error_log</p> | path | yes |  |  |
| level | <p>The log level for the main error log.</p><p>https://nginx.org/en/docs/ngx_core_module.html#error_log</p> | str | no | <ul><li>debug</li><li>info</li><li>notice</li><li>warn</li><li>error</li><li>crit</li><li>alert</li><li>emerg</li></ul> | error |

### Options for nginx_http_config_files
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| destination | <p>The path of the HTTP configuration file.</p> | path | yes |  |  |
| error_page | <p>The error page configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#error_page</p> | list of dicts of 'error_page' options | no |  |  |
| limit_rate | <p>The rate limit for the response.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#limit_rate</p> | str | no |  |  |
| limit_rate_after | <p>The number of bytes after which to apply the rate limit.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#limit_rate_after</p> | str | no |  |  |
| root | <p>The root directory for the server.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#root</p> | path | no |  |  |
| sendfile | <p>Whether to use the sendfile system call.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#sendfile</p> | bool | no |  |  |
| absolute_redirect | <p>Whether to use absolute URIs in redirects.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#absolute_redirect</p> | bool | no |  |  |
| aio | <p>Whether to use asynchronous I/O.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#aio</p> | bool | no |  |  |
| auth_delay | <p>The delay for authentication.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#auth_delay</p> | str | no |  |  |
| chunked_transfer_encoding | <p>Whether to use chunked transfer encoding.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#chunked_transfer_encoding</p> | bool | no |  |  |
| client_body_buffer_size | <p>The size of the client request body buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_buffer_size</p> | str | no |  |  |
| client_body_in_file_only | <p>Whether to store the client request body in a file.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_in_file_only</p> | str | no | <ul><li>on</li><li>clean</li><li>off</li></ul> |  |
| client_body_in_single_buffer | <p>Whether to store the client request body in a single buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_in_single_buffer</p> | bool | no |  |  |
| client_body_temp_path | <p>The client request body temporary storage configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_temp_path</p> | dict of 'client_body_temp_path' options | no |  |  |
| client_body_timeout | <p>The timeout for reading the client request body.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_timeout</p> | str | no |  |  |
| client_max_body_size | <p>The maximum size of the client request body.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_max_body_size</p> | str | no |  |  |
| default_type | <p>The default MIME type.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#default_type</p> | str | no |  |  |
| directio | <p>If set to `off`, disables the use of direct I/O.</p><p>If set to a size, enables the use of direct I/O with the specified size.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#directio</p> | str | no |  |  |
| directio_alignment | <p>The alignment for direct I/O.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#directio_alignment</p> | str | no |  |  |
| disable_symlinks | <p>The symbolic link configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#disable_symlinks</p> | dict of 'disable_symlinks' options | no |  |  |
| etag | <p>Whether to enable ETag generation.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#etag</p> | bool | no |  |  |
| if_modified_since | <p>The If-Modified-Since header configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#if_modified_since</p> | str | no | <ul><li>off</li><li>exact</li><li>before</li></ul> |  |
| keepalive_disable | <p>The keepalive disable configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_disable</p> | dict of 'keepalive_disable' options | no |  |  |
| keepalive_requests | <p>The maximum number of requests to allow on a keepalive connection.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_requests</p> | int | no |  |  |
| keepalive_time | <p>The time period over which to keep a keepalive connection open.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_time</p> | str | no |  |  |
| keepalive_timeout | <p>The timeout for keepalive connections.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_timeout</p> | dict of 'keepalive_timeout' options | no |  |  |
| lingering_close | <p>The lingering close configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#lingering_close</p> | str | no | <ul><li>off</li><li>on</li><li>always</li></ul> |  |
| lingering_time | <p>The time period over which to wait for more data.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#lingering_time</p> | str | no |  |  |
| lingering_timeout | <p>The timeout for lingering connections.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#lingering_timeout</p> | str | no |  |  |
| log_not_found | <p>Whether to log missing files.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#log_not_found</p> | bool | no |  |  |
| log_subrequest | <p>Whether to log subrequests.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#log_subrequest</p> | bool | no |  |  |
| max_ranges | <p>The maximum number of ranges to allow in a request.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#max_ranges</p> | int | no |  |  |
| msie_padding | <p>Whether to enable MSIE padding.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#msie_padding</p> | bool | no |  |  |
| msie_refresh | <p>Whether to enable MSIE refresh.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#msie_refresh</p> | bool | no |  |  |
| open_file_cache | <p>The open file cache configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache</p> | dict of 'open_file_cache' options | no |  |  |
| open_file_cache_errors | <p>Whether to cache errors.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache_errors</p> | bool | no |  |  |
| open_file_cache_min_uses | <p>The minimum number of uses to cache a file.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache_min_uses</p> | int | no |  |  |
| open_file_cache_valid | <p>The time period for which to cache a file.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache_valid</p> | str | no |  |  |
| output_buffers | <p>The output buffer configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#output_buffers</p> | dict of 'output_buffers' options | no |  |  |
| port_in_redirect | <p>Whether to include the port in redirects.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#port_in_redirect</p> | bool | no |  |  |
| postpone_output | <p>The number of bytes NGINX will wait for before sending data, if possible.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#postpone_output</p> | int | no |  |  |
| read_ahead | <p>The read ahead size configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#read_ahead</p> | str | no |  |  |
| recursive_error_pages | <p>Whether to enable recursive error pages.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#recursive_error_pages</p> | bool | no |  |  |
| reset_timedout_connection | <p>Whether to reset timed out connections.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#reset_timedout_connection</p> | bool | no |  |  |
| resolver | <p>The list of resolver configurations.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver</p> | list of dicts of 'resolver' options | no |  | [] |
| resolver_timeout | <p>The timeout for resolver queries.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver_timeout</p> | str | no |  |  |
| satisfy | <p>The satisfy configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#satisfy</p> | str | no | <ul><li>all</li><li>any</li></ul> |  |
| send_lowat | <p>The low water mark for sending data.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#send_lowat</p> | str | no |  |  |
| send_timeout | <p>The timeout for sending data.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#send_timeout</p> | str | no |  |  |
| sendfile_max_chunk | <p>The maximum size of a file chunk to send.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#sendfile_max_chunk</p> | str | no |  |  |
| server_name_in_redirect | <p>Whether to include the server name in redirects.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#server_name_in_redirect</p> | bool | no |  |  |
| server_tokens | <p>The server tokens configuration.</p><p>If set to `on`, the server version will be included in error messages.</p><p>If set to `off`, the server version will not be included in error messages.</p><p>If set to `build`, the server version will be included in error messages, but the NGINX version will not be included.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#server_tokens</p> | str | no | <ul><li>on</li><li>off</li><li>build</li></ul> |  |
| subrequest_output_buffer_size | <p>The size of the subrequest output buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#subrequest_output_buffer_size</p> | str | no |  |  |
| tcp_nodelay | <p>Whether to enable TCP_NODELAY.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#tcp_nodelay</p> | bool | no |  |  |
| tcp_nopush | <p>Whether to enable TCP_NOPUSH.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#tcp_nopush</p> | bool | no |  |  |
| types_hash_bucket_size | <p>The size of the types hash bucket.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#types_hash_bucket_size</p> | int | no |  |  |
| types_hash_max_size | <p>The maximum size of the types hash.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#types_hash_max_size</p> | int | no |  |  |
| types | <p>The list of MIME types mapped to file extensions.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#types</p> | list of dicts of 'types' options | no |  | [] |
| client_header_buffer_size | <p>The size of the client request header buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_header_buffer_size</p> | str | no |  |  |
| client_header_timeout | <p>The timeout for reading the client request header.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_header_timeout</p> | str | no |  |  |
| connection_pool_size | <p>The size of the connection pool.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#connection_pool_size</p> | int | no |  |  |
| ignore_invalid_headers | <p>Whether to ignore invalid headers.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#ignore_invalid_headers</p> | bool | no |  |  |
| large_client_header_buffers | <p>The large client header buffers configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#large_client_header_buffers</p> | dict of 'large_client_header_buffers' options | no |  |  |
| merge_slashes | <p>Whether to merge consecutive slashes in URIs.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#merge_slashes</p> | bool | no |  |  |
| request_pool_size | <p>The size of the request pool.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#request_pool_size</p> | str | no |  |  |
| underscores_in_headers | <p>Whether to allow underscores in headers.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#underscores_in_headers</p> | bool | no |  |  |
| server_names_hash_bucket_size | <p>The size of the server names hash bucket.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#server_names_hash_bucket_size</p> | int | no |  |  |
| server_names_hash_max_size | <p>The maximum size of the server names hash.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#server_names_hash_max_size</p> | int | no |  |  |
| variables_hash_bucket_size | <p>The size of the variables hash bucket.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#variables_hash_bucket_size</p> | int | no |  |  |
| variables_hash_max_size | <p>The maximum size of the variables hash.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#variables_hash_max_size</p> | int | no |  |  |
| http2_chunk_size | <p>The size of the HTTP/2 chunk.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_chunk_size</p> | str | no |  |  |
| http2_push | <p>The HTTP/2 push configuration.</p><p>If set to `off`, disables HTTP/2 push.</p><p>If set to a URI, enables HTTP/2 push for the specified URI.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_push</p> | str | no |  |  |
| http2_push_preload | <p>Whether to enable HTTP/2 push preload.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_push_preload</p> | bool | no |  |  |
| http2_body_preread_size | <p>The size of the HTTP/2 body preread.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_body_preread_size</p> | str | no |  |  |
| http2_idle_timeout | <p>The timeout for idle HTTP/2 connections.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_idle_timeout</p> | str | no |  |  |
| http2_max_concurrent_pushes | <p>The maximum number of concurrent HTTP/2 pushes.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_max_concurrent_pushes</p> | int | no |  |  |
| http2_max_concurrent_streams | <p>The maximum number of concurrent HTTP/2 streams.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_max_concurrent_streams</p> | int | no |  |  |
| http2_max_field_size | <p>The maximum size of an HTTP/2 field.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_max_field_size</p> | str | no |  |  |
| http2_max_header_size | <p>The maximum size of an HTTP/2 header.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_max_header_size</p> | str | no |  |  |
| http2_max_requests | <p>The maximum number of requests per HTTP/2 connection.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_max_requests</p> | int | no |  |  |
| http2_recv_buffer_size | <p>The size of the HTTP/2 receive buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_recv_buffer_size</p> | str | no |  |  |
| ssl_buffer_size | <p>The size of the SSL buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_buffer_size</p> | str | no |  |  |
| ssl_certificate | <p>The list of SSL certificates.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_certificate</p> | list of 'path' | no |  | [] |
| ssl_certificate_key | <p>The list of SSL certificate keys.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_certificate_key</p> | list of 'path' | no |  | [] |
| ssl_ciphers | <p>The SSL ciphers configuration in OpenSSL format.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_ciphers</p> | str | no |  |  |
| ssl_client_certificate | <p>The path to the SSL client certificate.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_client_certificate</p> | path | no |  |  |
| ssl_conf_command | <p>The list of SSL configuration commands.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_conf_command</p> | list of 'str' | no |  | [] |
| ssl_crl | <p>The path to the SSL certificate revocation list.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_crl</p> | path | no |  |  |
| ssl_dhparam | <p>The path to the SSL Diffie-Hellman parameter file.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_dhparam</p> | path | no |  |  |
| ssl_early_data | <p>Whether to enable SSL early data for TLS 1.3.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_early_data</p> | bool | no |  |  |
| ssl_ecdh_curve | <p>The SSL ECDH curve configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_ecdh_curve</p> | str | no |  |  |
| ssl_ocsp | <p>The SSL OCSP configuration.</p><p>If set to `off`, disables OCSP stapling.</p><p>If set to `on`, enables OCSP stapling for server and client certificates.</p><p>If set to `leaf`, enables OCSP stapling for client certificates only.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_ocsp</p> | str | no | <ul><li>on</li><li>off</li><li>leaf</li></ul> |  |
| ssl_ocsp_cache | <p>The SSL OCSP cache configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_ocsp_cache</p> | dict of 'ssl_ocsp_cache' options | no |  |  |
| ssl_password_file | <p>The path to the SSL password file.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_password_file</p> | path | no |  |  |
| ssl_prefer_server_ciphers | <p>Whether to prefer server ciphers.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_prefer_server_ciphers</p> | bool | no |  |  |
| ssl_protocols | <p>The SSL protocols configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_protocols</p> | dict of 'ssl_protocols' options | no |  |  |
| ssl_reject_handshake | <p>Whether to reject SSL handshakes for unknown server names.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_reject_handshake</p> | bool | no |  |  |
| ssl_session_cache | <p>The SSL session cache configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_cache</p> | dict of 'ssl_session_cache' options | no |  |  |
| ssl_session_ticket_key | <p>The list of SSL session ticket keys.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_ticket_key</p> | list of 'path' | no |  | [] |
| ssl_session_tickets | <p>Whether to enable SSL session tickets.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_tickets</p> | bool | no |  |  |
| ssl_session_timeout | <p>The timeout for SSL sessions.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_timeout</p> | str | no |  |  |
| ssl_stapling | <p>Whether to enable SSL stapling.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_stapling</p> | bool | no |  |  |
| ssl_stapling_file | <p>The path to the SSL stapling file.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_stapling_file</p> | path | no |  |  |
| ssl_stapling_responder | <p>The SSL stapling responder configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_stapling_responder</p> | str | no |  |  |
| ssl_stapling_verify | <p>Whether to verify SSL stapling responses.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_stapling_verify</p> | bool | no |  |  |
| ssl_trusted_certificate | <p>The path to the SSL trusted certificate.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_trusted_certificate</p> | path | no |  |  |
| ssl_verify_client | <p>The SSL client verification configuration.</p><p>If set to `off`, disables SSL client verification.</p><p>If set to `on`, enables SSL client verification.</p><p>If set to `optional`, enables SSL client verification, but does not require a client certificate.</p><p>If set to `optional_no_ca`, enables SSL client verification, but does not require a client certificate and does not verify the certificate.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_verify_client</p> | str | no | <ul><li>off</li><li>on</li><li>optional</li><li>optional_no_ca</li></ul> |  |
| ssl_verify_depth | <p>The maximum depth of the SSL certificate verification chain.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_verify_depth</p> | int | no |  |  |
| proxy_bind | <p>The proxy binding configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_bind</p> | dict of 'proxy_bind' options | no |  |  |
| proxy_buffer_size | <p>The size of the proxy buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffer_size</p> | str | no |  |  |
| proxy_buffering | <p>Whether to enable proxy buffering.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffering</p> | bool | no |  |  |
| proxy_buffers | <p>The proxy buffers configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffers</p> | dict of 'proxy_buffers' options | no |  |  |
| proxy_busy_buffers_size | <p>The size of the proxy busy buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_busy_buffers_size</p> | str | no |  |  |
| proxy_cache | <p>The proxy cache zone configuration.</p><p>If set to `off`, disables the proxy cache.</p><p>If set to a string, enables the proxy cache with the specified zone name.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache</p> | str | no |  |  |
| proxy_cache_background_update | <p>Whether to enable background updates for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_background_update</p> | bool | no |  |  |
| proxy_cache_bypass | <p>The list of proxy cache bypass rules.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_bypass</p> | list of 'str' | no |  | [] |
| proxy_cache_convert_head | <p>Whether to convert HEAD requests to GET requests for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_convert_head</p> | bool | no |  |  |
| proxy_cache_key | <p>The proxy cache key configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_key</p> | str | no |  |  |
| proxy_cache_lock | <p>Whether to enable locking for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_lock</p> | bool | no |  |  |
| proxy_cache_lock_age | <p>The time period after which to consider a lock stale.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_lock_age</p> | str | no |  |  |
| proxy_cache_lock_timeout | <p>The timeout for acquiring a lock for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_lock_timeout</p> | str | no |  |  |
| proxy_cache_max_range_offset | <p>The maximum range offset for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_max_range_offset</p> | int | no |  |  |
| proxy_cache_methods | <p>The list of methods to cache for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_methods</p> | list of 'str' | no | <ul><li>GET</li><li>HEAD</li><li>POST</li></ul> | [] |
| proxy_cache_min_uses | <p>The minimum number of uses to cache a response for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_min_uses</p> | int | no |  |  |
| proxy_cache_purge | <p>A list of conditions under which to purge the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_purge</p> | list of 'str' | no |  | [] |
| proxy_cache_revalidate | <p>Whether to enable revalidation for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_revalidate</p> | bool | no |  |  |
| proxy_cache_use_stale | <p>The configuration for the proxy_cache_use_stale directive.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_use_stale</p> | dict of 'proxy_cache_use_stale' options | no |  |  |
| proxy_cache_valid | <p>The proxy cache validity configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_valid</p> | list of dicts of 'proxy_cache_valid' options | no |  | [] |
| proxy_connect_timeout | <p>The timeout for connecting to the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_connect_timeout</p> | str | no |  |  |
| proxy_cookie_domain | <p>The proxy cookie domain configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_domain</p> | dict of 'proxy_cookie_domain' options | no |  |  |
| proxy_cookie_flags | <p>The proxy cookie flags configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_flags</p> | dict of 'proxy_cookie_flags' options | no |  |  |
| proxy_cookie_path | <p>The proxy cookie path configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_path</p> | dict of 'proxy_cookie_path' options | no |  |  |
| proxy_force_ranges | <p>Whether to force byte ranges for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_force_ranges</p> | bool | no |  |  |
| proxy_headers_hash_bucket_size | <p>The size of the proxy headers hash bucket.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_headers_hash_bucket_size</p> | int | no |  |  |
| proxy_headers_hash_max_size | <p>The maximum size of the proxy headers hash.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_headers_hash_max_size</p> | int | no |  |  |
| proxy_hide_header | <p>The list of headers to hide from the proxy response.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_hide_header</p> | list of 'str' | no |  | [] |
| proxy_http_version | <p>The HTTP version to use for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_http_version</p> | str | no | <ul><li>1.0</li><li>1.1</li></ul> |  |
| proxy_ignore_client_abort | <p>Whether to ignore client aborts for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ignore_client_abort</p> | bool | no |  |  |
| proxy_ignore_headers | <p>The list of headers to ignore from the proxy response.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ignore_headers</p> | list of 'str' | no | <ul><li>X-Accel-Redirect</li><li>X-Accel-Expires</li><li>X-Accel-Limit-Rate</li><li>X-Accel-Buffering</li><li>X-Accel-Charset</li><li>Expires</li><li>Cache-Control</li><li>Set-Cookie</li><li>Vary</li></ul> | [] |
| proxy_intercept_errors | <p>Whether to intercept errors for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_intercept_errors</p> | bool | no |  |  |
| proxy_limit_rate | <p>The rate limit for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_limit_rate</p> | str | no |  |  |
| proxy_max_temp_file_size | <p>The maximum size of a temporary file for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_max_temp_file_size</p> | str | no |  |  |
| proxy_method | <p>The method to use for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_method</p> | str | no |  |  |
| proxy_next_upstream | <p>The configuration for the proxy_next_upstream directive.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream</p> | dict of 'proxy_next_upstream' options | no |  |  |
| proxy_next_upstream_timeout | <p>The timeout for selecting the next upstream server for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream_timeout</p> | str | no |  |  |
| proxy_next_upstream_tries | <p>The number of tries to select the next upstream server for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream_tries</p> | int | no |  |  |
| proxy_no_cache | <p>The list of conditions under which to disable caching for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_no_cache</p> | list of 'str' | no |  | [] |
| proxy_pass_header | <p>The list of headers to pass to the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass_header</p> | list of 'str' | no |  | [] |
| proxy_pass_request_body | <p>Whether to pass the request</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass_request_body</p> | bool | no |  |  |
| proxy_pass_request_headers | <p>Whether to pass the request</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass_request_headers</p> | bool | no |  |  |
| proxy_read_timeout | <p>The timeout for reading from the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_read_timeout</p> | str | no |  |  |
| proxy_redirect | <p>The proxy redirect configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_redirect</p> | dict of 'proxy_redirect' options | no |  |  |
| proxy_request_buffering | <p>Whether to buffer the request for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_request_buffering</p> | bool | no |  |  |
| proxy_send_lowat | <p>The low water mark for sending data to the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_send_lowat</p> | str | no |  |  |
| proxy_send_timeout | <p>The timeout for sending data to the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_send_timeout</p> | str | no |  |  |
| proxy_set_body | <p>The list of body replacements for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_set_body</p> | str | no |  |  |
| proxy_set_header | <p>The list of headers to set for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_set_header</p> | list of dicts of 'proxy_set_header' options | no |  | [] |
| proxy_socket_keepalive | <p>Whether to enable socket keepalive for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_socket_keepalive</p> | bool | no |  |  |
| proxy_ssl_certificate | <p>The path to the SSL certificate for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_certificate</p> | path | no |  |  |
| proxy_ssl_certificate_key | <p>The path to the SSL certificate key for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_certificate_key</p> | path | no |  |  |
| proxy_ssl_ciphers | <p>The SSL ciphers configuration in OpenSSL format for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_ciphers</p> | str | no |  |  |
| proxy_ssl_conf_command | <p>The list of SSL configuration commands for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_conf_command</p> | list of 'str' | no |  | [] |
| proxy_ssl_crl | <p>The path to the SSL certificate revocation list for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_crl</p> | path | no |  |  |
| proxy_ssl_name | <p>The name to send for the SNI for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_name</p> | str | no |  |  |
| proxy_ssl_password_file | <p>The path to the SSL password file for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_password_file</p> | path | no |  |  |
| proxy_ssl_protocols | <p>The SSL protocols configuration for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | dict of 'proxy_ssl_protocols' options | no |  |  |
| proxy_ssl_server_name | <p>Whether to enable TLS Server Name Indication (SNI) for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_server_name</p> | bool | no |  |  |
| proxy_ssl_session_reuse | <p>Whether to enable SSL session reuse for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_session_reuse</p> | bool | no |  |  |
| proxy_ssl_trusted_certificate | <p>The path to the SSL trusted certificate for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_trusted_certificate</p> | path | no |  |  |
| proxy_ssl_verify | <p>Whether to enable SSL verification for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_verify</p> | bool | no |  |  |
| proxy_ssl_verify_depth | <p>The maximum depth of the SSL certificate verification chain for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_verify_depth</p> | int | no |  |  |
| proxy_store | <p>The path to store the proxy response.</p><p>If set to `off`, disables storing the proxy response.</p><p>If set to `on`, stores the proxy response in the default location.</p><p>If set to a path, stores the proxy response in the specified location.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_store</p> | str | no |  |  |
| proxy_store_access | <p>The file permissions for the stored proxy responses.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_store_access</p> | dict of 'proxy_store_access' options | no |  |  |
| proxy_temp_file_write_size | <p>The size of the temporary file for writing to the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_temp_file_write_size</p> | str | no |  |  |
| proxy_temp_path | <p>The proxy temporary path configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_temp_path</p> | dict of 'proxy_temp_path' options | no |  |  |
| proxy_cache_path | <p>The proxy cache path configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | list of dicts of 'proxy_cache_path' options | no |  | [] |
| grpc_bind | <p>The gRPC server bind configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_bind</p> | dict of 'grpc_bind' options | no |  |  |
| grpc_buffer_size | <p>The size of the gRPC buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_buffer_size</p> | str | no |  |  |
| grpc_connect_timeout | <p>The timeout for connecting to the gRPC server.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_connect_timeout</p> | str | no |  |  |
| grpc_hide_header | <p>The list of headers to hide from the gRPC response.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_hide_header</p> | list of 'str' | no |  | [] |
| grpc_ignore_headers | <p>The list of headers to ignore from the gRPC response.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ignore_headers</p> | list of 'str' | no | <ul><li>X-Accel-Redirect</li><li>X-Accel-Charset</li></ul> | [] |
| grpc_intercept_errors | <p>Whether to intercept errors for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_intercept_errors</p> | bool | no |  |  |
| grpc_next_upstream | <p>The list of conditions under which to select the next upstream server for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_next_upstream</p> | dict of 'grpc_next_upstream' options | no |  |  |
| grpc_next_upstream_timeout | <p>The timeout for selecting the next upstream server for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_next_upstream_timeout</p> | str | no |  |  |
| grpc_next_upstream_tries | <p>The number of tries to select the next upstream server for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_next_upstream_tries</p> | int | no |  |  |
| grpc_pass_header | <p>The list of headers to pass to the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_pass_header</p> | list of 'str' | no |  | [] |
| grpc_read_timeout | <p>The timeout for reading from the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_read_timeout</p> | str | no |  |  |
| grpc_send_timeout | <p>The timeout for sending data to the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_send_timeout</p> | str | no |  |  |
| grpc_set_header | <p>The list of headers to set for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_set_header</p> | list of dicts of 'grpc_set_header' options | no |  | [] |
| grpc_socket_keepalive | <p>Whether to enable socket keepalive for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_socket_keepalive</p> | bool | no |  |  |
| grpc_ssl_certificate | <p>The path to the SSL certificate for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_certificate</p> | path | no |  |  |
| grpc_ssl_certificate_key | <p>The path to the SSL certificate key for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_certificate_key</p> | path | no |  |  |
| grpc_ssl_ciphers | <p>The SSL ciphers configuration in OpenSSL format for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_ciphers</p> | str | no |  |  |
| grpc_ssl_conf_command | <p>The list of SSL configuration commands for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_conf_command</p> | list of 'str' | no |  | [] |
| grpc_ssl_crl | <p>The path to the SSL certificate revocation list for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_crl</p> | path | no |  |  |
| grpc_ssl_name | <p>The name to send for the SNI for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_name</p> | str | no |  |  |
| grpc_ssl_password_file | <p>The path to the SSL password file for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_password_file</p> | path | no |  |  |
| grpc_ssl_protocols | <p>The SSL protocols configuration for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | dict of 'grpc_ssl_protocols' options | no |  |  |
| grpc_ssl_server_name | <p>Whether to enable TLS Server Name Indication (SNI) for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_server_name</p> | bool | no |  |  |
| grpc_ssl_session_reuse | <p>Whether to enable SSL session reuse for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_session_reuse</p> | bool | no |  |  |
| grpc_ssl_trusted_certificate | <p>The path to the SSL trusted certificate for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_trusted_certificate</p> | path | no |  |  |
| grpc_ssl_verify | <p>Whether to enable SSL verification for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_verify</p> | bool | no |  |  |
| grpc_ssl_verify_depth | <p>The maximum depth of the SSL certificate verification chain for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_verify_depth</p> | int | no |  |  |
| allow | <p>The list of IP addresses or CIDR blocks to allow.</p><p>https://nginx.org/en/docs/http/ngx_http_access_module.html#allow</p> | list of 'str' | no |  | [] |
| deny | <p>The list of IP addresses or CIDR blocks to deny.</p><p>https://nginx.org/en/docs/http/ngx_http_access_module.html#deny</p> | list of 'str' | no |  | [] |
| auth_basic | <p>The basic authentication realm.</p><p>If set to `off`, disables basic authentication.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_basic_module.html#auth_basic</p> | str | no |  |  |
| auth_basic_user_file | <p>The path to the basic authentication user file.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_basic_module.html#auth_basic_user_file</p> | path | no |  |  |
| auth_request | <p>The URI to request for authentication.</p><p>If set to `off`, disables authentication requests.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_request_module.html#auth_request</p> | str | no |  |  |
| auth_request_set | <p>The list of variables to set from the authentication request.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_request_module.html#auth_request_set</p> | list of dicts of 'auth_request_set' options | no |  | [] |
| auth_jwt | <p>The JWT authentication configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt</p> | dict of 'auth_jwt' options | no |  |  |
| auth_jwt_key_file | <p>The path to the JWT key file.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_key_file</p> | path | no |  |  |
| auth_jwt_key_request | <p>The URI to request the JWT key.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_key_request</p> | str | no |  |  |
| auth_jwt_type | <p>The JWT type.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_type</p> | str | no | <ul><li>signed</li><li>encrypted</li><li>nested</li></ul> |  |
| auth_jwt_require | <p>The list of claims required in the JWT.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_require</p> | list of dicts of 'auth_jwt_require' options | no |  | [] |
| auth_jwt_key_cache | <p>The time to cache JWT keys.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_key_cache</p> | str | no |  |  |
| auth_jwt_leeway | <p>The leeway time for JWT verification.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_leeway</p> | str | no |  |  |
| auth_jwt_claim_set | <p>The list of claims to set from the JWT.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_claim_set</p> | list of dicts of 'auth_jwt_claim_set' options | no |  | [] |
| auth_jwt_header_set | <p>The list of headers to set from the JWT.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_header_set</p> | list of dicts of 'auth_jwt_header_set' options | no |  | [] |
| autoindex | <p>Whether to enable directory listing.</p><p>https://nginx.org/en/docs/http/ngx_http_autoindex_module.html#autoindex</p> | bool | no |  |  |
| autoindex_exact_size | <p>Whether to display the exact file size in directory listings.</p><p>https://nginx.org/en/docs/http/ngx_http_autoindex_module.html#autoindex_exact_size</p> | bool | no |  |  |
| autoindex_format | <p>The format of the directory listing.</p><p>https://nginx.org/en/docs/http/ngx_http_autoindex_module.html#autoindex_format</p> | str | no | <ul><li>html</li><li>xml</li><li>json</li><li>jsonp</li></ul> |  |
| autoindex_localtime | <p>Whether to display the file modification time in local time in directory listings.</p><p>https://nginx.org/en/docs/http/ngx_http_autoindex_module.html#autoindex_localtime</p> | bool | no |  |  |
| index | <p>The list of files to use as the index.</p><p>https://nginx.org/en/docs/http/ngx_http_index_module.html#index</p> | list of 'str' | no |  |  |
| random_index | <p>Whether to select a random file from the index.</p><p>https://nginx.org/en/docs/http/ngx_http_random_index_module.html#random_index</p> | bool | no |  |  |
| gunzip | <p>Whether to enable response decompression.</p><p>https://nginx.org/en/docs/http/ngx_http_gunzip_module.html#gunzip</p> | bool | no |  |  |
| gunzip_buffers | <p>The size and number of buffers for response decompression.</p><p>https://nginx.org/en/docs/http/ngx_http_gunzip_module.html#gunzip_buffers</p> | dict of 'gunzip_buffers' options | no |  |  |
| gzip | <p>Whether to enable response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip</p> | bool | no |  |  |
| gzip_buffers | <p>The size and number of buffers for response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_buffers</p> | dict of 'gzip_buffers' options | no |  |  |
| gzip_comp_level | <p>The compression level for response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_comp_level</p> | int | no |  |  |
| gzip_disable | <p>The list of user agents to disable response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_disable</p> | list of 'str' | no |  | [] |
| gzip_http_version | <p>The HTTP version for response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_http_version</p> | str | no | <ul><li>1.0</li><li>1.1</li></ul> |  |
| gzip_min_length | <p>The minimum length of the response to compress.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_min_length</p> | int | no |  |  |
| gzip_proxied | <p>The list of proxies for which to enable response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_proxied</p> | dict of 'gzip_proxied' options | no |  |  |
| gzip_types | <p>The list of MIME types to compress.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_types</p> | list of 'str' | no |  | [] |
| gzip_vary | <p>Whether to add the Vary header for response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_vary</p> | bool | no |  |  |
| gzip_static | <p>Whether to use precompressed files for response compression.</p><p>If set to `on`, uses precompressed files.</p><p>If set to `always`, uses precompressed files without checking client support.</p><p>If set to `off`, does not use precompressed files.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_static_module.html#gzip_static</p> | str | no | <ul><li>on</li><li>off</li><li>always</li></ul> |  |
| add_header | <p>The list of headers to add to the response.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_header</p> | list of dicts of 'add_header' options | no |  | [] |
| add_trailer | <p>The list of trailers to add to the response.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_trailer</p> | list of dicts of 'add_trailer' options | no |  | [] |
| expires | <p>The expiration configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | dict of 'expires' options | no |  |  |
| limit_req | <p>The request rate limiting configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req</p> | list of dicts of 'limit_req' options | no |  |  |
| limit_req_dry_run | <p>Whether to enable dry-run mode for rate limiting.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req_dry_run</p> | bool | no |  |  |
| limit_req_log_level | <p>The log level for rate limiting.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req_log_level</p> | str | no | <ul><li>info</li><li>notice</li><li>warn</li><li>error</li></ul> |  |
| limit_req_status | <p>The status code to return for requests exceeding the rate limit.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req_status</p> | int | no |  |  |
| limit_req_zone | <p>The list of shared memory zones for rate limiting.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req_zone</p> | list of dicts of 'limit_req_zone' options | no |  | [] |
| access_log | <p>The access log configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | dict of 'access_log' options | no |  |  |
| open_log_file_cache | <p>The open log file cache configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#open_log_file_cache</p> | dict of 'open_log_file_cache' options | no |  |  |
| error_log | <p>The error log configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#error_log</p> | dict of 'error_log' options | no |  |  |
| log_format | <p>The list of log formats.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#log_format</p> | list of dicts of 'log_format' options | no |  | [] |
| map_hash_bucket_size | <p>The size of the hash bucket for the map.</p><p>https://nginx.org/en/docs/http/ngx_http_map_module.html#map_hash_bucket_size</p> | int | no |  |  |
| map_hash_max_size | <p>The maximum size of the hash for the map.</p><p>https://nginx.org/en/docs/http/ngx_http_map_module.html#map_hash_max_size</p> | int | no |  |  |
| mirror | <p>The mirror configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_mirror_module.html#mirror</p> | dict of 'mirror' options | no |  |  |
| mirror_request_body | <p>Whether to mirror the request body.</p><p>https://nginx.org/en/docs/http/ngx_http_mirror_module.html#mirror_request_body</p> | bool | no |  |  |
| set_real_ip_from | <p>The IP address or CIDR block from which to set the real IP.</p><p>https://nginx.org/en/docs/http/ngx_http_realip_module.html#set_real_ip_from</p> | list of 'str' | no |  | [] |
| real_ip_header | <p>The header from which to set the real IP.</p><p>https://nginx.org/en/docs/http/ngx_http_realip_module.html#real_ip_header</p> | str | no |  |  |
| real_ip_recursive | <p>Whether to set the real IP recursively.</p><p>https://nginx.org/en/docs/http/ngx_http_realip_module.html#real_ip_recursive</p> | bool | no |  |  |
| rewrite_log | <p>Whether to enable rewrite logging.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite_log</p> | bool | no |  |  |
| uninitialized_variable_warn | <p>Whether to warn about uninitialized variables.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#uninitialized_variable_warn</p> | bool | no |  |  |
| sub_filter | <p>The list of substitutions to make in the response body.</p><p>https://nginx.org/en/docs/http/ngx_http_sub_module.html#sub_filter</p> | list of dicts of 'sub_filter' options | no |  | [] |
| sub_filter_last_modified | <p>Whether to replace the last modified time in the response body.</p><p>https://nginx.org/en/docs/http/ngx_http_sub_module.html#sub_filter_last_modified</p> | bool | no |  |  |
| sub_filter_once | <p>Whether to replace only the first match in the response body.</p><p>https://nginx.org/en/docs/http/ngx_http_sub_module.html#sub_filter_once</p> | bool | no |  |  |
| sub_filter_types | <p>The list of MIME types to replace in the response body.</p><p>https://nginx.org/en/docs/http/ngx_http_sub_module.html#sub_filter_types</p> | list of 'str' | no |  | [] |
| custom_directives | <p>The list of custom directives to include in the configuration.</p> | list of 'str' | no |  | [] |
| map | <p>The map configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_map_module.html#map</p> | list of dicts of 'map' options | no |  | [] |
| server | <p>The list of server blocks.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#server</p> | list of dicts of 'server' options | no |  | [] |
| split_clients | <p>The split clients configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_split_clients_module.html#split_clients</p> | list of dicts of 'split_clients' options | no |  | [] |
| upstream | <p>The list of upstream blocks.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#upstream</p> | list of dicts of 'upstream' options | no |  |  |

### Options for nginx_http_config_files > error_page
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| codes | <p>The list of error codes for which this directive applies.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#error_page</p> | list of 'int' | yes |  |  |
| response | <p>The response code configuration when the specified error codes would normally occur.</p><p>If not defined, the response code from the redirected URI is used.</p><p>If defined, this must be either `=` or `=XXX` with XXX being a response code.</p><p>If set to `=`, the original response code is retained.</p><p>If set to `=XXX`, the response code is replaced with `XXX`.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#error_page</p> | str | no |  |  |
| uri | <p>The URI of the error page.</p><p>Alternatively, this can be set to `@` followed by the name of a location block to reference it.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#error_page</p> | str | yes |  |  |

### Options for nginx_http_config_files > client_body_temp_path
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| path | <p>The path to the temporary storage directory.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_temp_path</p> | path | yes |  |  |
| level | <p>The number of subdirectories in the temporary storage directory.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_temp_path</p> | int | no | <ul><li>0</li><li>1</li><li>2</li><li>3</li></ul> | 0 |

### Options for nginx_http_config_files > disable_symlinks
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable symbolic links.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#disable_symlinks</p> | bool | no |  | False |
| if_not_owner | <p>Whether to disable symbolic links if the owner of the file is not the same as the owner of the link.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#disable_symlinks</p> | bool | no |  | False |
| from | <p>The source of the symbolic link.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#disable_symlinks</p> | str | no |  |  |

### Options for nginx_http_config_files > keepalive_disable
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| none | <p>Whether to disable keepalive connections.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_disable</p> | bool | no |  | False |
| browsers | <p>The list of browsers for which to disable keepalive connections.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_disable</p> | list of 'str' | no | <ul><li>msie6</li><li>safari</li></ul> | [] |

### Options for nginx_http_config_files > keepalive_timeout
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| timeout | <p>The time period for the keepalive timeout.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_timeout</p> | str | yes |  |  |
| header_timeout | <p>The time period for the keepalive header timeout.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_timeout</p> | str | no |  |  |

### Options for nginx_http_config_files > open_file_cache
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the open file cache.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache</p> | bool | no |  | False |
| max | <p>The maximum number of files to cache.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache</p> | int | no |  |  |
| inactive | <p>The time period after which to remove a file from the cache.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache</p> | str | no |  |  |

### Options for nginx_http_config_files > output_buffers
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| count | <p>The number of buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#output_buffers</p> | int | yes |  |  |
| size | <p>The size of the buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#output_buffers</p> | str | yes |  |  |

### Options for nginx_http_config_files > resolver
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| addresses | <p>The address of the resolver.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver</p> | list of 'str' | yes |  |  |
| valid | <p>The time period for which to cache a response.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver</p> | str | no |  |  |
| ipv4 | <p>Whether to resolve IPv4 addresses.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver</p> | bool | no |  | False |
| ipv6 | <p>Whether to resolve IPv6 addresses.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver</p> | bool | no |  | False |
| status_zone | <p>The shared memory zone for resolver status.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver</p> | str | no |  |  |

### Options for nginx_http_config_files > types
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| mime | <p>The MIME type.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#types</p> | str | yes |  |  |
| extensions | <p>The list of file extensions.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#types</p> | list of 'str' | yes |  |  |

### Options for nginx_http_config_files > large_client_header_buffers
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| count | <p>The number of buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#large_client_header_buffers</p> | int | yes |  |  |
| size | <p>The size of the buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#large_client_header_buffers</p> | str | yes |  |  |

### Options for nginx_http_config_files > ssl_ocsp_cache
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the OCSP cache.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_ocsp_cache</p> | bool | no |  | False |
| shared | <p>The shared memory zone for the OCSP cache.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_ocsp_cache</p> | dict of 'shared' options | no |  |  |

### Options for nginx_http_config_files > ssl_ocsp_cache > shared
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| name | <p>The name of the shared memory zone.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_ocsp_cache</p> | str | yes |  |  |
| size | <p>The size of the shared memory zone.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_ocsp_cache</p> | str | yes |  |  |

### Options for nginx_http_config_files > ssl_protocols
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| sslv2 | <p>Whether to enable SSLv2.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_protocols</p> | bool | no |  | False |
| sslv3 | <p>Whether to enable SSLv3.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_protocols</p> | bool | no |  | False |
| tlsv1 | <p>Whether to enable TLSv1.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_protocols</p> | bool | no |  | False |
| tlsv11 | <p>Whether to enable TLSv1.1.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_protocols</p> | bool | no |  | False |
| tlsv12 | <p>Whether to enable TLSv1.2.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_protocols</p> | bool | no |  | False |
| tlsv13 | <p>Whether to enable TLSv1.3.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_protocols</p> | bool | no |  | False |

### Options for nginx_http_config_files > ssl_session_cache
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the SSL session cache.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_cache</p> | bool | no |  | False |
| none | <p>Whether to allow session reuse and disable other the SSL session cache.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_cache</p> | bool | no |  | False |
| builtin | <p>The built-in SSL session cache configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_cache</p> | dict of 'builtin' options | no |  |  |
| shared | <p>The shared memory zone for the SSL session cache.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_cache</p> | dict of 'shared' options | no |  |  |

### Options for nginx_http_config_files > ssl_session_cache > builtin
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| size | <p>The size of the built-in SSL session cache.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_cache</p> | str | yes |  |  |

### Options for nginx_http_config_files > ssl_session_cache > shared
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| name | <p>The name of the shared memory zone.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_cache</p> | str | yes |  |  |
| size | <p>The size of the shared memory zone.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_cache</p> | str | yes |  |  |

### Options for nginx_http_config_files > proxy_bind
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable proxy binding.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_bind</p> | bool | no |  | False |
| address | <p>The address to which to bind the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_bind</p> | str | no |  |  |
| transparent | <p>Whether to enable transparent proxy binding.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_bind</p> | bool | no |  | False |

### Options for nginx_http_config_files > proxy_buffers
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| count | <p>The number of buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffers</p> | int | yes |  |  |
| size | <p>The size of the buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffers</p> | str | yes |  |  |

### Options for nginx_http_config_files > proxy_cache_use_stale
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the proxy_cache_use_stale directive.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_use_stale</p> | bool | no |  | False |
| conditions | <p>The list of conditions under which to use stale cache data.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_use_stale</p> | list of 'str' | no | <ul><li>error</li><li>timeout</li><li>invalid_header</li><li>updating</li><li>http_500</li><li>http_502</li><li>http_503</li><li>http_504</li><li>http_403</li><li>http_404</li><li>http_429</li></ul> |  |

### Options for nginx_http_config_files > proxy_cache_valid
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| codes | <p>The status codes for which to cache a response.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_valid</p> | list of 'int' | no |  |  |
| time | <p>The time period for which to cache a response.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_valid</p> | str | yes |  |  |

### Options for nginx_http_config_files > proxy_cookie_domain
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the proxy cookie domain.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_domain</p> | bool | no |  | False |
| domains | <p>The list of domain replacements.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_domain</p> | list of dicts of 'domains' options | no |  |  |

### Options for nginx_http_config_files > proxy_cookie_domain > domains
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| domain | <p>The domain to map.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_domain</p> | str | yes |  |  |
| replacement | <p>The domain replacement.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_domain</p> | str | yes |  |  |

### Options for nginx_http_config_files > proxy_cookie_flags
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the proxy cookie flags.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_flags</p> | bool | no |  | False |
| flags | <p>The list of cookie flags.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_flags</p> | list of dicts of 'flags' options | no |  |  |

### Options for nginx_http_config_files > proxy_cookie_flags > flags
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| cookie | <p>The name of the cookie.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_flags</p> | str | yes |  |  |
| flags | <p>The flags for the cookie.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_flags</p> | str | yes |  |  |

### Options for nginx_http_config_files > proxy_cookie_path
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the proxy cookie path.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_path</p> | bool | no |  | False |
| paths | <p>The list of path replacements.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_path</p> | list of dicts of 'paths' options | no |  |  |

### Options for nginx_http_config_files > proxy_cookie_path > paths
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| path | <p>The path to map.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_path</p> | str | yes |  |  |
| replacement | <p>The path replacement.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_path</p> | str | yes |  |  |

### Options for nginx_http_config_files > proxy_next_upstream
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the proxy_next_upstream directive.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream</p> | bool | no |  | False |
| conditions | <p>The list of conditions under which to select the next upstream server.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream</p> | list of 'str' | no | <ul><li>error</li><li>timeout</li><li>invalid_header</li><li>http_500</li><li>http_502</li><li>http_503</li><li>http_504</li><li>http_403</li><li>http_404</li><li>http_429</li><li>non_idempotent</li></ul> |  |

### Options for nginx_http_config_files > proxy_redirect
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable proxy redirects.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_redirect</p> | bool | no |  | False |
| default | <p>Whether to enable the default proxy redirect.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_redirect</p> | bool | no |  |  |
| redirects | <p>The list of redirect rules.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_redirect</p> | list of dicts of 'redirects' options | no |  |  |

### Options for nginx_http_config_files > proxy_redirect > redirects
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| original | <p>The redirect original.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_redirect</p> | str | yes |  |  |
| replacement | <p>The redirect replacement.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_redirect</p> | str | yes |  |  |

### Options for nginx_http_config_files > proxy_set_header
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| field | <p>The header field to set.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_set_header</p> | str | yes |  |  |
| value | <p>The header value to set.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_set_header</p> | str | yes |  |  |

### Options for nginx_http_config_files > proxy_ssl_protocols
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| sslv2 | <p>Whether to enable SSLv2.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | bool | no |  | False |
| sslv3 | <p>Whether to enable SSLv3.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | bool | no |  | False |
| tlsv1 | <p>Whether to enable TLSv1.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | bool | no |  | False |
| tlsv11 | <p>Whether to enable TLSv1.1.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | bool | no |  | False |
| tlsv12 | <p>Whether to enable TLSv1.2.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | bool | no |  | False |
| tlsv13 | <p>Whether to enable TLSv1.3.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | bool | no |  | False |

### Options for nginx_http_config_files > proxy_store_access
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| user | <p>The owner permissions in 'rwx' format for newly created files and directories.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_store_access</p> | str | yes |  |  |
| group | <p>The group permissions in 'rwx' format for newly created files and directories.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_store_access</p> | str | yes |  |  |
| all | <p>The general permissions in 'rwx' format for newly created files and directories.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_store_access</p> | str | yes |  |  |

### Options for nginx_http_config_files > proxy_temp_path
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| path | <p>The path to the temporary directory.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_temp_path</p> | path | yes |  |  |
| level | <p>The number of levels of subdirectories to create.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_temp_path</p> | int | no | <ul><li>0</li><li>1</li><li>2</li><li>3</li></ul> | 0 |

### Options for nginx_http_config_files > proxy_cache_path
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| path | <p>The path to the cache directory.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | path | yes |  |  |
| levels | <p>The number of levels of subdirectories to create.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | int | no | <ul><li>0</li><li>1</li><li>2</li></ul> | 0 |
| use_temp_path | <p>Whether to use the temporary path for the cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | bool | no |  |  |
| keys_zone | <p>The shared memory zone for the cache keys.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | dict of 'keys_zone' options | yes |  |  |
| inactive | <p>The time period after which to consider a response stale.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | str | no |  |  |
| max_size | <p>The maximum size of the cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | str | no |  |  |
| min_free | <p>The minimum amount of free space to maintain in the cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | str | no |  |  |
| manager_files | <p>The maximum number of cache manager files.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | int | no |  |  |
| manager_sleep | <p>The time period between cache manager operations.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | str | no |  |  |
| manager_threshold | <p>The minimum amount of free space to maintain in the cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | str | no |  |  |
| loader_files | <p>The maximum number of cache loader files.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | int | no |  |  |
| loader_sleep | <p>The time period between cache loader operations.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | str | no |  |  |
| loader_threshold | <p>The minimum amount of free space to maintain in the cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | str | no |  |  |
| purger | <p>Whether to enable the cache purger.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | bool | no |  |  |
| purger_files | <p>The maximum number of cache purger files.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | int | no |  |  |
| purger_sleep | <p>The time period between cache purger operations.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | str | no |  |  |
| purger_threshold | <p>The minimum amount of free space to maintain in the cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | str | no |  |  |

### Options for nginx_http_config_files > proxy_cache_path > keys_zone
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| name | <p>The name of the shared memory zone.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | str | yes |  |  |
| size | <p>The size of the shared memory zone.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path</p> | str | yes |  |  |

### Options for nginx_http_config_files > grpc_bind
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the gRPC server bind.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_bind</p> | bool | no |  | False |
| address | <p>The address to bind the gRPC server.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_bind</p> | str | yes |  |  |
| transparent | <p>Whether to enable transparent proxying.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_bind</p> | bool | no |  | False |

### Options for nginx_http_config_files > grpc_next_upstream
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the gRPC next upstream.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_next_upstream</p> | bool | no |  | False |
| conditions | <p>The list of conditions under which to select the next upstream server for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_next_upstream</p> | list of 'str' | no | <ul><li>error</li><li>timeout</li><li>invalid_header</li><li>http_500</li><li>http_502</li><li>http_503</li><li>http_504</li><li>http_403</li><li>http_404</li><li>http_429</li><li>non_idempotent</li></ul> |  |

### Options for nginx_http_config_files > grpc_set_header
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| field | <p>The header field to set.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_set_header</p> | str | yes |  |  |
| value | <p>The header value to set.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_set_header</p> | str | yes |  |  |

### Options for nginx_http_config_files > grpc_ssl_protocols
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| sslv2 | <p>Whether to enable SSLv2.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | bool | no |  | False |
| sslv3 | <p>Whether to enable SSLv3.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | bool | no |  | False |
| tlsv1 | <p>Whether to enable TLSv1.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | bool | no |  | False |
| tlsv11 | <p>Whether to enable TLSv1.1.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | bool | no |  | False |
| tlsv12 | <p>Whether to enable TLSv1.2.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | bool | no |  | False |
| tlsv13 | <p>Whether to enable TLSv1.3.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | bool | no |  | False |

### Options for nginx_http_config_files > auth_request_set
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| variable | <p>The variable to set.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_request_module.html#auth_request_set</p> | str | yes |  |  |
| value | <p>The value to set.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_request_module.html#auth_request_set</p> | str | yes |  |  |

### Options for nginx_http_config_files > auth_jwt
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable JWT authentication.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt</p> | bool | no |  | False |
| realm | <p>The JWT authentication realm.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt</p> | str | no |  |  |
| token | <p>The JWT token.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt</p> | str | no |  |  |

### Options for nginx_http_config_files > auth_jwt_require
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| values | <p>The list of values for the claim.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_require</p> | list of 'str' | yes |  |  |
| error | <p>The error code to return if the claim is missing.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_require</p> | int | no | <ul><li>401</li><li>403</li></ul> |  |

### Options for nginx_http_config_files > auth_jwt_claim_set
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| variable | <p>The variable to be set.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_claim_set</p> | str | yes |  |  |
| claims | <p>The claim values to which to set the variable.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_claim_set</p> | list of 'str' | yes |  |  |

### Options for nginx_http_config_files > auth_jwt_header_set
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| variable | <p>The variable to be set.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_header_set</p> | str | yes |  |  |
| claims | <p>The claim values to which to set the header.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_header_set</p> | list of 'str' | yes |  |  |

### Options for nginx_http_config_files > gunzip_buffers
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| number | <p>The number of buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_gunzip_module.html#gunzip_buffers</p> | int | yes |  |  |
| size | <p>The size of the buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_gunzip_module.html#gunzip_buffers</p> | str | yes |  |  |

### Options for nginx_http_config_files > gzip_buffers
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| number | <p>The number of buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_buffers</p> | int | yes |  |  |
| size | <p>The size of the buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_buffers</p> | str | yes |  |  |

### Options for nginx_http_config_files > gzip_proxied
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable response compression for all proxies.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_proxied</p> | bool | no |  | False |
| any | <p>Whether to enable response compression for any proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_proxied</p> | bool | no |  | False |
| conditions | <p>The list of proxies for which to enable response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_proxied</p> | list of 'str' | no | <ul><li>expired</li><li>no-cache</li><li>no-store</li><li>private</li><li>no_last_modified</li><li>no_etag</li><li>auth</li></ul> |  |

### Options for nginx_http_config_files > add_header
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| field | <p>The header field to add.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_header</p> | str | yes |  |  |
| value | <p>The header value to add.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_header</p> | str | yes |  |  |
| always | <p>Whether to add the header to all responses.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_header</p> | bool | no |  | False |

### Options for nginx_http_config_files > add_trailer
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| field | <p>The trailer field to add.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_trailer</p> | str | yes |  |  |
| value | <p>The trailer value to add.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_trailer</p> | str | yes |  |  |
| always | <p>Whether to add the trailer to all responses.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_trailer</p> | bool | no |  | False |

### Options for nginx_http_config_files > expires
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable expiration.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | bool | no |  | False |
| max | <p>Whether to set the expiration to the maximum value.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | bool | no |  | False |
| epoch | <p>Whether to set the expiration to the minimum value and disable caching that way.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | bool | no |  | False |
| modified | <p>Whether to set the expiration relative to the last modification time.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | bool | no |  |  |
| time | <p>The time to set the expiration.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | str | no |  |  |

### Options for nginx_http_config_files > limit_req
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| zone | <p>The shared memory zone for rate limiting.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req</p> | str | yes |  |  |
| burst | <p>The maximum burst size.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req</p> | int | no |  |  |
| nodelay | <p>Whether to delay requests exceeding the rate limit.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req</p> | bool | no |  | False |
| delay | <p>The time period to delay requests exceeding the rate limit.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req</p> | str | no |  |  |

### Options for nginx_http_config_files > limit_req_zone
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| key | <p>The key for the shared memory zone.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req_zone</p> | str | yes |  |  |
| name | <p>The name of the shared memory zone.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req_zone</p> | str | yes |  |  |
| size | <p>The size of the shared memory zone.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req_zone</p> | str | yes |  |  |
| rate | <p>The rate limit for the shared memory zone.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req_zone</p> | str | yes |  |  |

### Options for nginx_http_config_files > access_log
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable access logging.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | bool | no |  | False |
| logs | <p>The list of access log files.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | list of dicts of 'logs' options | no |  |  |

### Options for nginx_http_config_files > access_log > logs
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| path | <p>The path to the access log file.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | path | yes |  |  |
| format | <p>The log format.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |
| buffer | <p>The buffer size for the access log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |
| gzip | <p>The gzip configuration for the access log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | dict of 'gzip' options | no |  |  |
| flush | <p>The time period to flush the access log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |
| if | <p>The condition under which to log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |

### Options for nginx_http_config_files > access_log > logs > gzip
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| level | <p>The compression level.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | int | no |  |  |

### Options for nginx_http_config_files > open_log_file_cache
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the open log file cache.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#open_log_file_cache</p> | bool | no |  | False |
| max | <p>The maximum number of open log files to cache.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#open_log_file_cache</p> | int | no |  |  |
| inactive | <p>The time period to keep an open log file in the cache.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#open_log_file_cache</p> | str | no |  |  |
| min_uses | <p>The minimum number of uses for an open log file to keep it in the cache.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#open_log_file_cache</p> | int | no |  |  |
| valid | <p>The time period to consider a cached open log file valid.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#open_log_file_cache</p> | str | no |  |  |

### Options for nginx_http_config_files > error_log
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| file | <p>The path to the error log file.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#error_log</p> | path | no |  |  |
| level | <p>The log level for the error log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#error_log</p> | str | no | <ul><li>debug</li><li>info</li><li>notice</li><li>warn</li><li>error</li><li>crit</li><li>alert</li><li>emerg</li></ul> |  |

### Options for nginx_http_config_files > log_format
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| name | <p>The name of the log format.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#log_format</p> | str | yes |  |  |
| escape | <p>The escape character for the log format.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#log_format</p> | str | no | <ul><li>default</li><li>json</li><li>none</li></ul> |  |
| format | <p>The log format.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#log_format</p> | str | yes |  |  |

### Options for nginx_http_config_files > mirror
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable mirroring.</p><p>https://nginx.org/en/docs/http/ngx_http_mirror_module.html#mirror</p> | bool | no |  | False |
| uris | <p>The list of URIs to mirror.</p><p>https://nginx.org/en/docs/http/ngx_http_mirror_module.html#mirror</p> | list of 'str' | no |  |  |

### Options for nginx_http_config_files > sub_filter
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| pattern | <p>The pattern to match.</p><p>https://nginx.org/en/docs/http/ngx_http_sub_module.html#sub_filter</p> | str | yes |  |  |
| replacement | <p>The replacement value.</p><p>https://nginx.org/en/docs/http/ngx_http_sub_module.html#sub_filter</p> | str | yes |  |  |

### Options for nginx_http_config_files > map
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| string | <p>The variable from which to map.</p><p>https://nginx.org/en/docs/http/ngx_http_map_module.html#map</p> | str | yes |  |  |
| variable | <p>The varaible to which to map.</p><p>https://nginx.org/en/docs/http/ngx_http_map_module.html#map</p> | str | no |  |  |
| hostnames | <p>Whether to map hostnames.</p><p>https://nginx.org/en/docs/http/ngx_http_map_module.html#map</p> | bool | no |  | False |
| volatile | <p>Whether the map is volatile.</p><p>https://nginx.org/en/docs/http/ngx_http_map_module.html#map</p> | bool | no |  | False |
| content | <p>The list of map content.</p><p>https://nginx.org/en/docs/http/ngx_http_map_module.html#map</p> | list of dicts of 'content' options | no |  |  |

### Options for nginx_http_config_files > map > content
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| match | <p>The key to map.</p><p>https://nginx.org/en/docs/http/ngx_http_map_module.html#map</p> | str | yes |  |  |
| value | <p>The value to map.</p><p>https://nginx.org/en/docs/http/ngx_http_map_module.html#map</p> | str | yes |  |  |

### Options for nginx_http_config_files > server
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| error_page | <p>The error page configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#error_page</p> | list of dicts of 'error_page' options | no |  |  |
| limit_rate | <p>The rate limit for the response.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#limit_rate</p> | str | no |  |  |
| limit_rate_after | <p>The number of bytes after which to apply the rate limit.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#limit_rate_after</p> | str | no |  |  |
| root | <p>The root directory for the server.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#root</p> | path | no |  |  |
| sendfile | <p>Whether to use the sendfile system call.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#sendfile</p> | bool | no |  |  |
| absolute_redirect | <p>Whether to use absolute URIs in redirects.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#absolute_redirect</p> | bool | no |  |  |
| aio | <p>Whether to use asynchronous I/O.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#aio</p> | bool | no |  |  |
| auth_delay | <p>The delay for authentication.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#auth_delay</p> | str | no |  |  |
| chunked_transfer_encoding | <p>Whether to use chunked transfer encoding.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#chunked_transfer_encoding</p> | bool | no |  |  |
| client_body_buffer_size | <p>The size of the client request body buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_buffer_size</p> | str | no |  |  |
| client_body_in_file_only | <p>Whether to store the client request body in a file.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_in_file_only</p> | str | no | <ul><li>on</li><li>clean</li><li>off</li></ul> |  |
| client_body_in_single_buffer | <p>Whether to store the client request body in a single buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_in_single_buffer</p> | bool | no |  |  |
| client_body_temp_path | <p>The client request body temporary storage configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_temp_path</p> | dict of 'client_body_temp_path' options | no |  |  |
| client_body_timeout | <p>The timeout for reading the client request body.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_timeout</p> | str | no |  |  |
| client_max_body_size | <p>The maximum size of the client request body.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_max_body_size</p> | str | no |  |  |
| default_type | <p>The default MIME type.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#default_type</p> | str | no |  |  |
| directio | <p>If set to `off`, disables the use of direct I/O.</p><p>If set to a size, enables the use of direct I/O with the specified size.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#directio</p> | str | no |  |  |
| directio_alignment | <p>The alignment for direct I/O.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#directio_alignment</p> | str | no |  |  |
| disable_symlinks | <p>The symbolic link configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#disable_symlinks</p> | dict of 'disable_symlinks' options | no |  |  |
| etag | <p>Whether to enable ETag generation.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#etag</p> | bool | no |  |  |
| if_modified_since | <p>The If-Modified-Since header configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#if_modified_since</p> | str | no | <ul><li>off</li><li>exact</li><li>before</li></ul> |  |
| keepalive_disable | <p>The keepalive disable configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_disable</p> | dict of 'keepalive_disable' options | no |  |  |
| keepalive_requests | <p>The maximum number of requests to allow on a keepalive connection.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_requests</p> | int | no |  |  |
| keepalive_time | <p>The time period over which to keep a keepalive connection open.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_time</p> | str | no |  |  |
| keepalive_timeout | <p>The timeout for keepalive connections.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_timeout</p> | dict of 'keepalive_timeout' options | no |  |  |
| lingering_close | <p>The lingering close configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#lingering_close</p> | str | no | <ul><li>off</li><li>on</li><li>always</li></ul> |  |
| lingering_time | <p>The time period over which to wait for more data.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#lingering_time</p> | str | no |  |  |
| lingering_timeout | <p>The timeout for lingering connections.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#lingering_timeout</p> | str | no |  |  |
| log_not_found | <p>Whether to log missing files.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#log_not_found</p> | bool | no |  |  |
| log_subrequest | <p>Whether to log subrequests.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#log_subrequest</p> | bool | no |  |  |
| max_ranges | <p>The maximum number of ranges to allow in a request.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#max_ranges</p> | int | no |  |  |
| msie_padding | <p>Whether to enable MSIE padding.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#msie_padding</p> | bool | no |  |  |
| msie_refresh | <p>Whether to enable MSIE refresh.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#msie_refresh</p> | bool | no |  |  |
| open_file_cache | <p>The open file cache configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache</p> | dict of 'open_file_cache' options | no |  |  |
| open_file_cache_errors | <p>Whether to cache errors.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache_errors</p> | bool | no |  |  |
| open_file_cache_min_uses | <p>The minimum number of uses to cache a file.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache_min_uses</p> | int | no |  |  |
| open_file_cache_valid | <p>The time period for which to cache a file.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache_valid</p> | str | no |  |  |
| output_buffers | <p>The output buffer configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#output_buffers</p> | dict of 'output_buffers' options | no |  |  |
| port_in_redirect | <p>Whether to include the port in redirects.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#port_in_redirect</p> | bool | no |  |  |
| postpone_output | <p>The number of bytes NGINX will wait for before sending data, if possible.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#postpone_output</p> | int | no |  |  |
| read_ahead | <p>The read ahead size configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#read_ahead</p> | str | no |  |  |
| recursive_error_pages | <p>Whether to enable recursive error pages.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#recursive_error_pages</p> | bool | no |  |  |
| reset_timedout_connection | <p>Whether to reset timed out connections.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#reset_timedout_connection</p> | bool | no |  |  |
| resolver | <p>The list of resolver configurations.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver</p> | list of dicts of 'resolver' options | no |  | [] |
| resolver_timeout | <p>The timeout for resolver queries.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver_timeout</p> | str | no |  |  |
| satisfy | <p>The satisfy configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#satisfy</p> | str | no | <ul><li>all</li><li>any</li></ul> |  |
| send_lowat | <p>The low water mark for sending data.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#send_lowat</p> | str | no |  |  |
| send_timeout | <p>The timeout for sending data.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#send_timeout</p> | str | no |  |  |
| sendfile_max_chunk | <p>The maximum size of a file chunk to send.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#sendfile_max_chunk</p> | str | no |  |  |
| server_name_in_redirect | <p>Whether to include the server name in redirects.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#server_name_in_redirect</p> | bool | no |  |  |
| server_tokens | <p>The server tokens configuration.</p><p>If set to `on`, the server version will be included in error messages.</p><p>If set to `off`, the server version will not be included in error messages.</p><p>If set to `build`, the server version will be included in error messages, but the NGINX version will not be included.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#server_tokens</p> | str | no | <ul><li>on</li><li>off</li><li>build</li></ul> |  |
| subrequest_output_buffer_size | <p>The size of the subrequest output buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#subrequest_output_buffer_size</p> | str | no |  |  |
| tcp_nodelay | <p>Whether to enable TCP_NODELAY.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#tcp_nodelay</p> | bool | no |  |  |
| tcp_nopush | <p>Whether to enable TCP_NOPUSH.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#tcp_nopush</p> | bool | no |  |  |
| types_hash_bucket_size | <p>The size of the types hash bucket.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#types_hash_bucket_size</p> | int | no |  |  |
| types_hash_max_size | <p>The maximum size of the types hash.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#types_hash_max_size</p> | int | no |  |  |
| client_header_buffer_size | <p>The size of the client request header buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_header_buffer_size</p> | str | no |  |  |
| client_header_timeout | <p>The timeout for reading the client request header.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_header_timeout</p> | str | no |  |  |
| connection_pool_size | <p>The size of the connection pool.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#connection_pool_size</p> | int | no |  |  |
| ignore_invalid_headers | <p>Whether to ignore invalid headers.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#ignore_invalid_headers</p> | bool | no |  |  |
| large_client_header_buffers | <p>The large client header buffers configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#large_client_header_buffers</p> | dict of 'large_client_header_buffers' options | no |  |  |
| merge_slashes | <p>Whether to merge consecutive slashes in URIs.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#merge_slashes</p> | bool | no |  |  |
| request_pool_size | <p>The size of the request pool.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#request_pool_size</p> | str | no |  |  |
| underscores_in_headers | <p>Whether to allow underscores in headers.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#underscores_in_headers</p> | bool | no |  |  |
| try_files | <p>The list of files to try.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#try_files</p> | list of dicts of 'try_files' options | no |  | [] |
| listen | <p>The list of listen configurations.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | list of dicts of 'listen' options | no |  | [] |
| server_name | <p>The list of server names.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#server_name</p> | list of 'str' | no |  | [] |
| http2_chunk_size | <p>The size of the HTTP/2 chunk.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_chunk_size</p> | str | no |  |  |
| http2_push | <p>The HTTP/2 push configuration.</p><p>If set to `off`, disables HTTP/2 push.</p><p>If set to a URI, enables HTTP/2 push for the specified URI.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_push</p> | str | no |  |  |
| http2_push_preload | <p>Whether to enable HTTP/2 push preload.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_push_preload</p> | bool | no |  |  |
| http2_body_preread_size | <p>The size of the HTTP/2 body preread.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_body_preread_size</p> | str | no |  |  |
| http2_idle_timeout | <p>The timeout for idle HTTP/2 connections.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_idle_timeout</p> | str | no |  |  |
| http2_max_concurrent_pushes | <p>The maximum number of concurrent HTTP/2 pushes.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_max_concurrent_pushes</p> | int | no |  |  |
| http2_max_concurrent_streams | <p>The maximum number of concurrent HTTP/2 streams.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_max_concurrent_streams</p> | int | no |  |  |
| http2_max_field_size | <p>The maximum size of an HTTP/2 field.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_max_field_size</p> | str | no |  |  |
| http2_max_header_size | <p>The maximum size of an HTTP/2 header.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_max_header_size</p> | str | no |  |  |
| http2_max_requests | <p>The maximum number of requests per HTTP/2 connection.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_max_requests</p> | int | no |  |  |
| ssl_buffer_size | <p>The size of the SSL buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_buffer_size</p> | str | no |  |  |
| ssl_certificate | <p>The list of SSL certificates.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_certificate</p> | list of 'path' | no |  | [] |
| ssl_certificate_key | <p>The list of SSL certificate keys.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_certificate_key</p> | list of 'path' | no |  | [] |
| ssl_ciphers | <p>The SSL ciphers configuration in OpenSSL format.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_ciphers</p> | str | no |  |  |
| ssl_client_certificate | <p>The path to the SSL client certificate.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_client_certificate</p> | path | no |  |  |
| ssl_conf_command | <p>The list of SSL configuration commands.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_conf_command</p> | list of 'str' | no |  | [] |
| ssl_crl | <p>The path to the SSL certificate revocation list.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_crl</p> | path | no |  |  |
| ssl_dhparam | <p>The path to the SSL Diffie-Hellman parameter file.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_dhparam</p> | path | no |  |  |
| ssl_early_data | <p>Whether to enable SSL early data for TLS 1.3.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_early_data</p> | bool | no |  |  |
| ssl_ecdh_curve | <p>The SSL ECDH curve configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_ecdh_curve</p> | str | no |  |  |
| ssl_ocsp | <p>The SSL OCSP configuration.</p><p>If set to `off`, disables OCSP stapling.</p><p>If set to `on`, enables OCSP stapling for server and client certificates.</p><p>If set to `leaf`, enables OCSP stapling for client certificates only.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_ocsp</p> | str | no | <ul><li>on</li><li>off</li><li>leaf</li></ul> |  |
| ssl_ocsp_cache | <p>The SSL OCSP cache configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_ocsp_cache</p> | dict of 'ssl_ocsp_cache' options | no |  |  |
| ssl_password_file | <p>The path to the SSL password file.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_password_file</p> | path | no |  |  |
| ssl_prefer_server_ciphers | <p>Whether to prefer server ciphers.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_prefer_server_ciphers</p> | bool | no |  |  |
| ssl_protocols | <p>The SSL protocols configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_protocols</p> | dict of 'ssl_protocols' options | no |  |  |
| ssl_reject_handshake | <p>Whether to reject SSL handshakes for unknown server names.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_reject_handshake</p> | bool | no |  |  |
| ssl_session_cache | <p>The SSL session cache configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_cache</p> | dict of 'ssl_session_cache' options | no |  |  |
| ssl_session_ticket_key | <p>The list of SSL session ticket keys.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_ticket_key</p> | list of 'path' | no |  | [] |
| ssl_session_tickets | <p>Whether to enable SSL session tickets.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_tickets</p> | bool | no |  |  |
| ssl_session_timeout | <p>The timeout for SSL sessions.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_timeout</p> | str | no |  |  |
| ssl_stapling | <p>Whether to enable SSL stapling.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_stapling</p> | bool | no |  |  |
| ssl_stapling_file | <p>The path to the SSL stapling file.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_stapling_file</p> | path | no |  |  |
| ssl_stapling_responder | <p>The SSL stapling responder configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_stapling_responder</p> | str | no |  |  |
| ssl_stapling_verify | <p>Whether to verify SSL stapling responses.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_stapling_verify</p> | bool | no |  |  |
| ssl_trusted_certificate | <p>The path to the SSL trusted certificate.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_trusted_certificate</p> | path | no |  |  |
| ssl_verify_client | <p>The SSL client verification configuration.</p><p>If set to `off`, disables SSL client verification.</p><p>If set to `on`, enables SSL client verification.</p><p>If set to `optional`, enables SSL client verification, but does not require a client certificate.</p><p>If set to `optional_no_ca`, enables SSL client verification, but does not require a client certificate and does not verify the certificate.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_verify_client</p> | str | no | <ul><li>off</li><li>on</li><li>optional</li><li>optional_no_ca</li></ul> |  |
| ssl_verify_depth | <p>The maximum depth of the SSL certificate verification chain.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_verify_depth</p> | int | no |  |  |
| proxy_bind | <p>The proxy binding configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_bind</p> | dict of 'proxy_bind' options | no |  |  |
| proxy_buffer_size | <p>The size of the proxy buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffer_size</p> | str | no |  |  |
| proxy_buffers | <p>The proxy buffers configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffers</p> | dict of 'proxy_buffers' options | no |  |  |
| proxy_busy_buffers_size | <p>The size of the proxy busy buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_busy_buffers_size</p> | str | no |  |  |
| proxy_cache | <p>The proxy cache zone configuration.</p><p>If set to `off`, disables the proxy cache.</p><p>If set to a string, enables the proxy cache with the specified zone name.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache</p> | str | no |  |  |
| proxy_cache_background_update | <p>Whether to enable background updates for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_background_update</p> | bool | no |  |  |
| proxy_cache_bypass | <p>The list of proxy cache bypass rules.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_bypass</p> | list of 'str' | no |  | [] |
| proxy_cache_convert_head | <p>Whether to convert HEAD requests to GET requests for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_convert_head</p> | bool | no |  |  |
| proxy_cache_key | <p>The proxy cache key configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_key</p> | str | no |  |  |
| proxy_cache_lock | <p>Whether to enable locking for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_lock</p> | bool | no |  |  |
| proxy_cache_lock_age | <p>The time period after which to consider a lock stale.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_lock_age</p> | str | no |  |  |
| proxy_cache_lock_timeout | <p>The timeout for acquiring a lock for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_lock_timeout</p> | str | no |  |  |
| proxy_cache_max_range_offset | <p>The maximum range offset for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_max_range_offset</p> | str | no |  |  |
| proxy_cache_methods | <p>The list of methods to cache for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_methods</p> | list of 'str' | no | <ul><li>GET</li><li>HEAD</li><li>POST</li></ul> | [] |
| proxy_cache_min_uses | <p>The minimum number of uses to cache a response for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_min_uses</p> | int | no |  |  |
| proxy_cache_purge | <p>A list of conditions under which to purge the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_purge</p> | list of 'str' | no |  | [] |
| proxy_cache_revalidate | <p>Whether to enable revalidation for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_revalidate</p> | bool | no |  |  |
| proxy_cache_use_stale | <p>The configuration for the proxy_cache_use_stale directive.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_use_stale</p> | list of dicts of 'proxy_cache_use_stale' options | no |  |  |
| proxy_cache_valid | <p>The proxy cache validity configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_valid</p> | list of dicts of 'proxy_cache_valid' options | no |  | [] |
| proxy_connect_timeout | <p>The timeout for connecting to the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_connect_timeout</p> | str | no |  |  |
| proxy_cookie_domain | <p>The proxy cookie domain configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_domain</p> | dict of 'proxy_cookie_domain' options | no |  |  |
| proxy_cookie_flags | <p>The proxy cookie flags configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_flags</p> | dict of 'proxy_cookie_flags' options | no |  |  |
| proxy_cookie_path | <p>The proxy cookie path configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_path</p> | dict of 'proxy_cookie_path' options | no |  |  |
| proxy_force_ranges | <p>Whether to force byte ranges for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_force_ranges</p> | bool | no |  |  |
| proxy_headers_hash_bucket_size | <p>The size of the proxy headers hash bucket.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_headers_hash_bucket_size</p> | int | no |  |  |
| proxy_headers_hash_max_size | <p>The maximum size of the proxy headers hash.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_headers_hash_max_size</p> | int | no |  |  |
| proxy_hide_header | <p>The list of headers to hide from the proxy response.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_hide_header</p> | list of 'str' | no |  | [] |
| proxy_http_version | <p>The HTTP version to use for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_http_version</p> | str | no | <ul><li>1.0</li><li>1.1</li></ul> |  |
| proxy_ignore_client_abort | <p>Whether to ignore client aborts for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ignore_client_abort</p> | bool | no |  |  |
| proxy_ignore_headers | <p>The list of headers to ignore from the proxy response.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ignore_headers</p> | list of 'str' | no | <ul><li>X-Accel-Redirect</li><li>X-Accel-Expires</li><li>X-Accel-Limit-Rate</li><li>X-Accel-Buffering</li><li>X-Accel-Charset</li><li>Expires</li><li>Cache-Control</li><li>Set-Cookie</li><li>Vary</li></ul> | [] |
| proxy_intercept_errors | <p>Whether to intercept errors for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_intercept_errors</p> | bool | no |  |  |
| proxy_limit_rate | <p>The rate limit for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_limit_rate</p> | str | no |  |  |
| proxy_max_temp_file_size | <p>The maximum size of a temporary file for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_max_temp_file_size</p> | str | no |  |  |
| proxy_method | <p>The method to use for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_method</p> | str | no |  |  |
| proxy_next_upstream | <p>The configuration for the proxy_next_upstream directive.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream</p> | list of dicts of 'proxy_next_upstream' options | no |  | [] |
| proxy_next_upstream_timeout | <p>The timeout for selecting the next upstream server for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream_timeout</p> | str | no |  |  |
| proxy_next_upstream_tries | <p>The number of tries to select the next upstream server for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream_tries</p> | int | no |  |  |
| proxy_no_cache | <p>The list of conditions under which to disable caching for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_no_cache</p> | list of 'str' | no |  | [] |
| proxy_pass_header | <p>The list of headers to pass to the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass_header</p> | list of 'str' | no |  | [] |
| proxy_pass_request_body | <p>Whether to pass the request</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass_request_body</p> | bool | no |  |  |
| proxy_pass_request_headers | <p>Whether to pass the request</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass_request_headers</p> | bool | no |  |  |
| proxy_read_timeout | <p>The timeout for reading from the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_read_timeout</p> | str | no |  |  |
| proxy_redirect | <p>The proxy redirect configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_redirect</p> | dict of 'proxy_redirect' options | no |  |  |
| proxy_request_buffering | <p>Whether to buffer the request for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_request_buffering</p> | bool | no |  |  |
| proxy_send_lowat | <p>The low water mark for sending data to the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_send_lowat</p> | str | no |  |  |
| proxy_send_timeout | <p>The timeout for sending data to the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_send_timeout</p> | str | no |  |  |
| proxy_set_body | <p>The list of body replacements for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_set_body</p> | str | no |  |  |
| proxy_set_header | <p>The list of headers to set for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_set_header</p> | list of dicts of 'proxy_set_header' options | no |  | [] |
| proxy_socket_keepalive | <p>Whether to enable socket keepalive for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_socket_keepalive</p> | bool | no |  |  |
| proxy_ssl_certificate | <p>The path to the SSL certificate for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_certificate</p> | path | no |  |  |
| proxy_ssl_certificate_key | <p>The path to the SSL certificate key for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_certificate_key</p> | path | no |  |  |
| proxy_ssl_ciphers | <p>The SSL ciphers configuration in OpenSSL format for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_ciphers</p> | str | no |  |  |
| proxy_ssl_conf_command | <p>The list of SSL configuration commands for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_conf_command</p> | list of 'str' | no |  | [] |
| proxy_ssl_crl | <p>The path to the SSL certificate revocation list for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_crl</p> | path | no |  |  |
| proxy_ssl_name | <p>The name to send for the SNI for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_name</p> | str | no |  |  |
| proxy_ssl_password_file | <p>The path to the SSL password file for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_password_file</p> | path | no |  |  |
| proxy_ssl_protocols | <p>The SSL protocols configuration for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | dict of 'proxy_ssl_protocols' options | no |  |  |
| proxy_ssl_server_name | <p>Whether to enable TLS Server Name Indication (SNI) for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_server_name</p> | bool | no |  |  |
| proxy_ssl_session_reuse | <p>Whether to enable SSL session reuse for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_session_reuse</p> | bool | no |  |  |
| proxy_ssl_trusted_certificate | <p>The path to the SSL trusted certificate for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_trusted_certificate</p> | path | no |  |  |
| proxy_ssl_verify | <p>Whether to enable SSL verification for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_verify</p> | bool | no |  |  |
| proxy_ssl_verify_depth | <p>The maximum depth of the SSL certificate verification chain for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_verify_depth</p> | int | no |  |  |
| proxy_store | <p>The path to store the proxy response.</p><p>If set to `off`, disables storing the proxy response.</p><p>If set to `on`, stores the proxy response in the default location.</p><p>If set to a path, stores the proxy response in the specified location.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_store</p> | str | no |  |  |
| proxy_store_access | <p>The file permissions for the stored proxy responses.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_store_access</p> | dict of 'proxy_store_access' options | no |  |  |
| proxy_temp_file_write_size | <p>The size of the temporary file for writing to the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_temp_file_write_size</p> | str | no |  |  |
| proxy_temp_path | <p>The proxy temporary path configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_temp_path</p> | dict of 'proxy_temp_path' options | no |  |  |
| grpc_bind | <p>The gRPC server bind configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_bind</p> | dict of 'grpc_bind' options | no |  |  |
| grpc_buffer_size | <p>The size of the gRPC buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_buffer_size</p> | str | no |  |  |
| grpc_connect_timeout | <p>The timeout for connecting to the gRPC server.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_connect_timeout</p> | str | no |  |  |
| grpc_hide_header | <p>The list of headers to hide from the gRPC response.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_hide_header</p> | list of 'str' | no |  | [] |
| grpc_ignore_headers | <p>The list of headers to ignore from the gRPC response.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ignore_headers</p> | list of 'str' | no | <ul><li>X-Accel-Redirect</li><li>X-Accel-Charset</li></ul> | [] |
| grpc_intercept_errors | <p>Whether to intercept errors for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_intercept_errors</p> | bool | no |  |  |
| grpc_next_upstream | <p>The list of conditions under which to select the next upstream server for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_next_upstream</p> | dict of 'grpc_next_upstream' options | no |  |  |
| grpc_next_upstream_timeout | <p>The timeout for selecting the next upstream server for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_next_upstream_timeout</p> | str | no |  |  |
| grpc_next_upstream_tries | <p>The number of tries to select the next upstream server for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_next_upstream_tries</p> | int | no |  |  |
| grpc_pass_header | <p>The list of headers to pass to the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_pass_header</p> | list of 'str' | no |  | [] |
| grpc_read_timeout | <p>The timeout for reading from the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_read_timeout</p> | str | no |  |  |
| grpc_send_timeout | <p>The timeout for sending data to the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_send_timeout</p> | str | no |  |  |
| grpc_set_header | <p>The list of headers to set for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_set_header</p> | list of dicts of 'grpc_set_header' options | no |  | [] |
| grpc_socket_keepalive | <p>Whether to enable socket keepalive for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_socket_keepalive</p> | bool | no |  |  |
| grpc_ssl_certificate | <p>The path to the SSL certificate for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_certificate</p> | path | no |  |  |
| grpc_ssl_certificate_key | <p>The path to the SSL certificate key for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_certificate_key</p> | path | no |  |  |
| grpc_ssl_ciphers | <p>The SSL ciphers configuration in OpenSSL format for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_ciphers</p> | str | no |  |  |
| grpc_ssl_conf_command | <p>The list of SSL configuration commands for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_conf_command</p> | list of 'str' | no |  | [] |
| grpc_ssl_crl | <p>The path to the SSL certificate revocation list for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_crl</p> | path | no |  |  |
| grpc_ssl_name | <p>The name to send for the SNI for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_name</p> | str | no |  |  |
| grpc_ssl_password_file | <p>The path to the SSL password file for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_password_file</p> | path | no |  |  |
| grpc_ssl_protocols | <p>The SSL protocols configuration for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | dict of 'grpc_ssl_protocols' options | no |  |  |
| grpc_ssl_server_name | <p>Whether to enable TLS Server Name Indication (SNI) for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_server_name</p> | bool | no |  |  |
| grpc_ssl_session_reuse | <p>Whether to enable SSL session reuse for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_session_reuse</p> | bool | no |  |  |
| grpc_ssl_trusted_certificate | <p>The path to the SSL trusted certificate for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_trusted_certificate</p> | path | no |  |  |
| grpc_ssl_verify | <p>Whether to enable SSL verification for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_verify</p> | bool | no |  |  |
| grpc_ssl_verify_depth | <p>The maximum depth of the SSL certificate verification chain for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_verify_depth</p> | int | no |  |  |
| allow | <p>The list of IP addresses or CIDR blocks to allow.</p><p>https://nginx.org/en/docs/http/ngx_http_access_module.html#allow</p> | list of 'str' | no |  | [] |
| deny | <p>The list of IP addresses or CIDR blocks to deny.</p><p>https://nginx.org/en/docs/http/ngx_http_access_module.html#deny</p> | list of 'str' | no |  | [] |
| auth_basic | <p>The basic authentication realm.</p><p>If set to `off`, disables basic authentication.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_basic_module.html#auth_basic</p> | str | no |  |  |
| auth_basic_user_file | <p>The path to the basic authentication user file.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_basic_module.html#auth_basic_user_file</p> | path | no |  |  |
| auth_request | <p>The URI to request for authentication.</p><p>If set to `off`, disables authentication requests.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_request_module.html#auth_request</p> | str | no |  |  |
| auth_request_set | <p>The list of variables to set from the authentication request.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_request_module.html#auth_request_set</p> | list of dicts of 'auth_request_set' options | no |  | [] |
| auth_jwt | <p>The JWT authentication configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt</p> | dict of 'auth_jwt' options | no |  |  |
| auth_jwt_key_file | <p>The path to the JWT key file.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_key_file</p> | path | no |  |  |
| auth_jwt_key_request | <p>The URI to request the JWT key.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_key_request</p> | str | no |  |  |
| auth_jwt_type | <p>The JWT type.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_type</p> | str | no | <ul><li>signed</li><li>encrypted</li><li>nested</li></ul> |  |
| auth_jwt_require | <p>The list of claims required in the JWT.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_require</p> | list of dicts of 'auth_jwt_require' options | no |  | [] |
| auth_jwt_key_cache | <p>The time to cache JWT keys.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_key_cache</p> | str | no |  |  |
| auth_jwt_leeway | <p>The leeway time for JWT verification.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_leeway</p> | str | no |  |  |
| stub_status | <p>Whether to enable the stub status module.</p><p>https://nginx.org/en/docs/http/ngx_http_stub_status_module.html#stub_status</p> | bool | no |  | False |
| autoindex | <p>Whether to enable directory listing.</p><p>https://nginx.org/en/docs/http/ngx_http_autoindex_module.html#autoindex</p> | bool | no |  |  |
| autoindex_exact_size | <p>Whether to display the exact file size in directory listings.</p><p>https://nginx.org/en/docs/http/ngx_http_autoindex_module.html#autoindex_exact_size</p> | bool | no |  |  |
| autoindex_format | <p>The format of the directory listing.</p><p>https://nginx.org/en/docs/http/ngx_http_autoindex_module.html#autoindex_format</p> | str | no | <ul><li>html</li><li>xml</li><li>json</li><li>jsonp</li></ul> |  |
| autoindex_localtime | <p>Whether to display the file modification time in local time in directory listings.</p><p>https://nginx.org/en/docs/http/ngx_http_autoindex_module.html#autoindex_localtime</p> | bool | no |  |  |
| index | <p>The list of files to use as the index.</p><p>https://nginx.org/en/docs/http/ngx_http_index_module.html#index</p> | list of 'str' | no |  |  |
| random_index | <p>Whether to select a random file from the index.</p><p>https://nginx.org/en/docs/http/ngx_http_random_index_module.html#random_index</p> | bool | no |  |  |
| gunzip | <p>Whether to enable response decompression.</p><p>https://nginx.org/en/docs/http/ngx_http_gunzip_module.html#gunzip</p> | bool | no |  |  |
| gunzip_buffers | <p>The size and number of buffers for response decompression.</p><p>https://nginx.org/en/docs/http/ngx_http_gunzip_module.html#gunzip_buffers</p> | dict of 'gunzip_buffers' options | no |  |  |
| gzip | <p>Whether to enable response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip</p> | bool | no |  |  |
| gzip_buffers | <p>The size and number of buffers for response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_buffers</p> | dict of 'gzip_buffers' options | no |  |  |
| gzip_disable | <p>The list of user agents to disable response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_disable</p> | list of 'str' | no |  | [] |
| gzip_http_version | <p>The HTTP version for response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_http_version</p> | str | no | <ul><li>1.0</li><li>1.1</li></ul> |  |
| gzip_min_length | <p>The minimum length of the response to compress.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_min_length</p> | str | no |  |  |
| gzip_proxied | <p>The list of proxies for which to enable response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_proxied</p> | dict of 'gzip_proxied' options | no |  |  |
| gzip_types | <p>The list of MIME types to compress.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_types</p> | list of 'str' | no |  | [] |
| gzip_vary | <p>Whether to add the Vary header for response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_vary</p> | bool | no |  |  |
| gzip_static | <p>Whether to use precompressed files for response compression.</p><p>If set to `on`, uses precompressed files.</p><p>If set to `always`, uses precompressed files without checking client support.</p><p>If set to `off`, does not use precompressed files.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_static_module.html#gzip_static</p> | str | no | <ul><li>on</li><li>off</li><li>always</li></ul> |  |
| add_header | <p>The list of headers to add to the response.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_header</p> | list of dicts of 'add_header' options | no |  | [] |
| add_trailer | <p>The list of trailers to add to the response.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_trailer</p> | list of dicts of 'add_trailer' options | no |  | [] |
| expires | <p>The expiration configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | dict of 'expires' options | no |  |  |
| limit_req | <p>The request rate limiting configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req</p> | list of dicts of 'limit_req' options | no |  |  |
| limit_req_dry_run | <p>Whether to enable dry-run mode for rate limiting.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req_dry_run</p> | bool | no |  |  |
| limit_req_log_level | <p>The log level for rate limiting.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req_log_level</p> | str | no | <ul><li>info</li><li>notice</li><li>warn</li><li>error</li></ul> |  |
| limit_req_status | <p>The status code to return for requests exceeding the rate limit.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req_status</p> | int | no |  |  |
| access_log | <p>The access log configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | dict of 'access_log' options | no |  |  |
| open_log_file_cache | <p>The open log file cache configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#open_log_file_cache</p> | dict of 'open_log_file_cache' options | no |  |  |
| error_log | <p>The error log configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#error_log</p> | dict of 'error_log' options | no |  |  |
| mirror | <p>The mirror configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_mirror_module.html#mirror</p> | dict of 'mirror' options | no |  |  |
| mirror_request_body | <p>Whether to mirror the request body.</p><p>https://nginx.org/en/docs/http/ngx_http_mirror_module.html#mirror_request_body</p> | bool | no |  |  |
| set_real_ip_from | <p>The IP address or CIDR block from which to set the real IP.</p><p>https://nginx.org/en/docs/http/ngx_http_realip_module.html#set_real_ip_from</p> | list of 'str' | no |  | [] |
| real_ip_header | <p>The header from which to set the real IP.</p><p>https://nginx.org/en/docs/http/ngx_http_realip_module.html#real_ip_header</p> | str | no |  |  |
| real_ip_recursive | <p>Whether to set the real IP recursively.</p><p>https://nginx.org/en/docs/http/ngx_http_realip_module.html#real_ip_recursive</p> | bool | no |  |  |
| rewrite_log | <p>Whether to enable rewrite logging.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite_log</p> | bool | no |  |  |
| uninitialized_variable_warn | <p>Whether to warn about uninitialized variables.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#uninitialized_variable_warn</p> | bool | no |  |  |
| break | <p>Whether to break rewrite processing.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#break</p> | bool | no |  |  |
| return | <p>The return configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#return</p> | dict of 'return' options | no |  |  |
| rewrite | <p>The list of rewrite rules.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite</p> | list of dicts of 'rewrite' options | no |  | [] |
| set | <p>The list of variables to set.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#set</p> | list of dicts of 'set' options | no |  | [] |
| sub_filter | <p>The list of substitutions to make in the response body.</p><p>https://nginx.org/en/docs/http/ngx_http_sub_module.html#sub_filter</p> | list of dicts of 'sub_filter' options | no |  | [] |
| sub_filter_last_modified | <p>Whether to replace the last modified time in the response body.</p><p>https://nginx.org/en/docs/http/ngx_http_sub_module.html#sub_filter_last_modified</p> | bool | no |  |  |
| sub_filter_once | <p>Whether to replace only the first match in the response body.</p><p>https://nginx.org/en/docs/http/ngx_http_sub_module.html#sub_filter_once</p> | bool | no |  |  |
| sub_filter_types | <p>The list of MIME types to replace in the response body.</p><p>https://nginx.org/en/docs/http/ngx_http_sub_module.html#sub_filter_types</p> | list of 'str' | no |  | [] |
| custom_directives | <p>The list of custom directives to include in the configuration.</p> | list of 'str' | no |  | [] |
| location | <p>The list of location blocks.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#location</p> | list of dicts of 'location' options | no |  | [] |
| if | <p>The list of if blocks.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#if</p> | list of dicts of 'if' options | no |  | [] |

### Options for nginx_http_config_files > server > error_page
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| codes | <p>The list of error codes for which this directive applies.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#error_page</p> | list of 'int' | yes |  |  |
| response | <p>The response code configuration when the specified error codes would normally occur.</p><p>If not defined, the response code from the redirected URI is used.</p><p>If defined, this must be either `=` or `=XXX` with XXX being a response code.</p><p>If set to `=`, the original response code is retained.</p><p>If set to `=XXX`, the response code is replaced with `XXX`.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#error_page</p> | str | no |  |  |
| uri | <p>The URI of the error page.</p><p>Alternatively, this can be set to `@` followed by the name of a location block to reference it.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#error_page</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > client_body_temp_path
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| path | <p>The path to the temporary storage directory.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_temp_path</p> | path | yes |  |  |
| level | <p>The number of subdirectories in the temporary storage directory.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_temp_path</p> | int | no | <ul><li>0</li><li>1</li><li>2</li><li>3</li></ul> | 0 |

### Options for nginx_http_config_files > server > disable_symlinks
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable symbolic links.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#disable_symlinks</p> | bool | no |  | False |
| if_not_owner | <p>Whether to disable symbolic links if the owner of the file is not the same as the owner of the link.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#disable_symlinks</p> | bool | no |  | False |
| from | <p>The source of the symbolic link.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#disable_symlinks</p> | str | no |  |  |

### Options for nginx_http_config_files > server > keepalive_disable
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| none | <p>Whether to disable keepalive connections.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_disable</p> | bool | no |  | False |
| browsers | <p>The list of browsers for which to disable keepalive connections.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_disable</p> | list of 'str' | no |  | [] |

### Options for nginx_http_config_files > server > keepalive_timeout
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| timeout | <p>The time period for the keepalive timeout.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_timeout</p> | str | yes |  |  |
| header_timeout | <p>The time period for the keepalive header timeout.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_timeout</p> | str | no |  |  |

### Options for nginx_http_config_files > server > open_file_cache
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the open file cache.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache</p> | bool | no |  | False |
| max | <p>The maximum number of files to cache.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache</p> | int | no |  |  |
| inactive | <p>The time period after which to remove a file from the cache.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache</p> | str | no |  |  |

### Options for nginx_http_config_files > server > output_buffers
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| count | <p>The number of buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#output_buffers</p> | int | yes |  |  |
| size | <p>The size of the buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#output_buffers</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > resolver
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| addresses | <p>The address of the resolver.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver</p> | list of 'str' | yes |  |  |
| valid | <p>The time period for which to cache a response.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver</p> | str | no |  |  |
| ipv4 | <p>Whether to resolve IPv4 addresses.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver</p> | bool | no |  | False |
| ipv6 | <p>Whether to resolve IPv6 addresses.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver</p> | bool | no |  | False |
| status_zone | <p>The shared memory zone for resolver status.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver</p> | str | no |  |  |

### Options for nginx_http_config_files > server > large_client_header_buffers
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| count | <p>The number of buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#large_client_header_buffers</p> | int | yes |  |  |
| size | <p>The size of the buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#large_client_header_buffers</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > try_files
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| files | <p>The list of files to try.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#try_files</p> | list of 'str' | yes |  |  |
| response | <p>The response to send if the file is not found.</p><p>This can either be a URI or an error code.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#try_files</p> | str | no |  |  |

### Options for nginx_http_config_files > server > listen
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| address | <p>The address on which to listen for connections.</p><p>If not supplied, this defaults to a wildcard address.</p><p>Either this and/or the `port` parameter must be supplied.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | str | no |  |  |
| port | <p>The port on which to listen for connections.</p><p>This defaults to port 80, if not supplied.</p><p>Either this and/or the `address` parameter must be supplied.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | int | no |  |  |
| default_server | <p>Whether to mark this as the default server.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | bool | no |  | False |
| ssl | <p>Whether to enable SSL.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | bool | no |  | False |
| http2 | <p>Whether to enable HTTP/2.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | bool | no |  | False |
| proxy_protocol | <p>Whether to enable the PROXY protocol.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | bool | no |  | False |
| setfib | <p>The setfib value.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | int | no |  |  |
| fastopen | <p>The fastopen value.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | int | no |  |  |
| backlog | <p>The backlog value.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | int | no |  |  |
| rcvbuf | <p>The receive buffer size value.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | str | no |  |  |
| sndbuf | <p>The send buffer size value.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | str | no |  |  |
| accept_filter | <p>The name of the accept filter.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | str | no |  |  |
| deferred | <p>Whether to enable deferred accept.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | bool | no |  | False |
| bind | <p>The address to bind to.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | str | no |  |  |
| ipv6only | <p>Whether to enable IPv6 only.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | bool | no |  |  |
| reuseport | <p>Whether to enable SO_REUSEPORT.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | bool | no |  | False |
| so_keepalive | <p>The SO_KEEPALIVE configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | dict of 'so_keepalive' options | no |  |  |

### Options for nginx_http_config_files > server > listen > so_keepalive
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable SO_KEEPALIVE.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | bool | no |  | False |
| on | <p>Whether to enable SO_KEEPALIVE with default parameters.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | bool | no |  | False |
| keepidle | <p>The keepalive idle time.</p><p>At least one of `keepidle`, `keepintvl`, or `keepcnt` must be specified, if `on` and `off` are `false`.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | str | no |  |  |
| keepintvl | <p>The keepalive interval.</p><p>At least one of `keepidle`, `keepintvl`, or `keepcnt` must be specified, if `on` and `off` are `false`.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | str | no |  |  |
| keepcnt | <p>The keepalive count.</p><p>At least one of `keepidle`, `keepintvl`, or `keepcnt` must be specified, if `on` and `off` are `false`.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#listen</p> | int | no |  |  |

### Options for nginx_http_config_files > server > ssl_ocsp_cache
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the OCSP cache.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_ocsp_cache</p> | bool | no |  | False |
| shared | <p>The shared memory zone for the OCSP cache.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_ocsp_cache</p> | dict of 'shared' options | no |  |  |

### Options for nginx_http_config_files > server > ssl_ocsp_cache > shared
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| name | <p>The name of the shared memory zone.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_ocsp_cache</p> | str | yes |  |  |
| size | <p>The size of the shared memory zone.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_ocsp_cache</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > ssl_protocols
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| sslv2 | <p>Whether to enable SSLv2.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_protocols</p> | bool | no |  | False |
| sslv3 | <p>Whether to enable SSLv3.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_protocols</p> | bool | no |  | False |
| tlsv1 | <p>Whether to enable TLSv1.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_protocols</p> | bool | no |  | False |
| tlsv11 | <p>Whether to enable TLSv1.1.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_protocols</p> | bool | no |  | False |
| tlsv12 | <p>Whether to enable TLSv1.2.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_protocols</p> | bool | no |  | False |
| tlsv13 | <p>Whether to enable TLSv1.3.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_protocols</p> | bool | no |  | False |

### Options for nginx_http_config_files > server > ssl_session_cache
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the SSL session cache.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_cache</p> | bool | no |  | False |
| none | <p>Whether to allow session reuse and disable other the SSL session cache.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_cache</p> | bool | no |  | False |
| builtin | <p>The built-in SSL session cache configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_cache</p> | dict of 'builtin' options | no |  |  |
| shared | <p>The shared memory zone for the SSL session cache.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_cache</p> | dict of 'shared' options | no |  |  |

### Options for nginx_http_config_files > server > ssl_session_cache > builtin
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| size | <p>The size of the built-in SSL session cache.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_cache</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > ssl_session_cache > shared
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| name | <p>The name of the shared memory zone.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_cache</p> | str | yes |  |  |
| size | <p>The size of the shared memory zone.</p><p>https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_session_cache</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > proxy_bind
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable proxy binding.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_bind</p> | bool | no |  | False |
| address | <p>The address to which to bind the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_bind</p> | str | no |  |  |
| transparent | <p>Whether to enable transparent proxy binding.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_bind</p> | bool | no |  | False |

### Options for nginx_http_config_files > server > proxy_buffers
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| count | <p>The number of buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffers</p> | int | yes |  |  |
| size | <p>The size of the buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffers</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > proxy_cache_use_stale
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the proxy_cache_use_stale directive.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_use_stale</p> | bool | no |  | False |
| conditions | <p>The list of conditions under which to use stale cache data.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_use_stale</p> | list of 'str' | no | <ul><li>error</li><li>timeout</li><li>invalid_header</li><li>updating</li><li>http_500</li><li>http_502</li><li>http_503</li><li>http_504</li><li>http_403</li><li>http_404</li><li>http_429</li></ul> |  |

### Options for nginx_http_config_files > server > proxy_cache_valid
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| codes | <p>The status codes for which to cache a response.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_valid</p> | list of 'int' | yes |  |  |
| time | <p>The time period for which to cache a response.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_valid</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > proxy_cookie_domain
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the proxy cookie domain.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_domain</p> | bool | no |  | False |
| domains | <p>The list of domain replacements.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_domain</p> | list of dicts of 'domains' options | no |  |  |

### Options for nginx_http_config_files > server > proxy_cookie_domain > domains
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| domain | <p>The domain to map.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_domain</p> | str | yes |  |  |
| replacement | <p>The domain replacement.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_domain</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > proxy_cookie_flags
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the proxy cookie flags.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_flags</p> | bool | no |  | False |
| flags | <p>The list of cookie flags.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_flags</p> | list of dicts of 'flags' options | no |  |  |

### Options for nginx_http_config_files > server > proxy_cookie_flags > flags
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| cookie | <p>The name of the cookie.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_flags</p> | str | yes |  |  |
| flags | <p>The flags for the cookie.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_flags</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > proxy_cookie_path
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the proxy cookie path.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_path</p> | bool | no |  | False |
| paths | <p>The list of path replacements.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_path</p> | list of dicts of 'paths' options | no |  |  |

### Options for nginx_http_config_files > server > proxy_cookie_path > paths
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| path | <p>The path to map.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_path</p> | str | yes |  |  |
| replacement | <p>The path replacement.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_path</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > proxy_next_upstream
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the proxy_next_upstream directive.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream</p> | bool | no |  | False |
| conditions | <p>The list of conditions under which to select the next upstream server.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream</p> | list of 'str' | no | <ul><li>error</li><li>timeout</li><li>invalid_header</li><li>http_500</li><li>http_502</li><li>http_503</li><li>http_504</li><li>http_403</li><li>http_404</li><li>http_429</li><li>non_idempotent</li></ul> |  |

### Options for nginx_http_config_files > server > proxy_redirect
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable proxy redirects.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_redirect</p> | bool | no |  | False |
| default | <p>Whether to enable the default proxy redirect.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_redirect</p> | bool | no |  |  |
| redirects | <p>The list of redirect rules.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_redirect</p> | list of dicts of 'redirects' options | no |  |  |

### Options for nginx_http_config_files > server > proxy_redirect > redirects
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| original | <p>The redirect original.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_redirect</p> | str | yes |  |  |
| replacement | <p>The redirect replacement.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_redirect</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > proxy_set_header
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| field | <p>The header field to set.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_set_header</p> | str | yes |  |  |
| value | <p>The header value to set.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_set_header</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > proxy_ssl_protocols
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| sslv2 | <p>Whether to enable SSLv2.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | bool | no |  | False |
| sslv3 | <p>Whether to enable SSLv3.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | bool | no |  | False |
| tlsv1 | <p>Whether to enable TLSv1.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | bool | no |  | False |
| tlsv11 | <p>Whether to enable TLSv1.1.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | bool | no |  | False |
| tlsv12 | <p>Whether to enable TLSv1.2.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | bool | no |  | False |
| tlsv13 | <p>Whether to enable TLSv1.3.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | bool | no |  | False |

### Options for nginx_http_config_files > server > proxy_store_access
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| user | <p>The owner permissions in 'rwx' format for newly created files and directories.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_store_access</p> | str | yes |  |  |
| group | <p>The group permissions in 'rwx' format for newly created files and directories.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_store_access</p> | str | yes |  |  |
| all | <p>The general permissions in 'rwx' format for newly created files and directories.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_store_access</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > proxy_temp_path
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| path | <p>The path to the temporary directory.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_temp_path</p> | path | yes |  |  |
| level | <p>The number of levels of subdirectories to create.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_temp_path</p> | int | no | <ul><li>0</li><li>1</li><li>2</li><li>3</li></ul> | 0 |

### Options for nginx_http_config_files > server > grpc_bind
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the gRPC server bind.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_bind</p> | bool | no |  | False |
| address | <p>The address to bind the gRPC server.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_bind</p> | str | yes |  |  |
| transparent | <p>Whether to enable transparent proxying.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_bind</p> | bool | no |  | False |

### Options for nginx_http_config_files > server > grpc_next_upstream
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the gRPC next upstream.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_next_upstream</p> | bool | no |  | False |
| upstreams | <p>The list of conditions under which to select the next upstream server for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_next_upstream</p> | list of 'str' | no | <ul><li>error</li><li>timeout</li><li>invalid_header</li><li>http_500</li><li>http_502</li><li>http_503</li><li>http_504</li><li>http_403</li><li>http_404</li><li>http_429</li><li>non_idempotent</li></ul> |  |

### Options for nginx_http_config_files > server > grpc_set_header
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| field | <p>The header field to set.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_set_header</p> | str | yes |  |  |
| value | <p>The header value to set.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_set_header</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > grpc_ssl_protocols
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| sslv2 | <p>Whether to enable SSLv2.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | bool | no |  | False |
| sslv3 | <p>Whether to enable SSLv3.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | bool | no |  | False |
| tlsv1 | <p>Whether to enable TLSv1.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | bool | no |  | False |
| tlsv11 | <p>Whether to enable TLSv1.1.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | bool | no |  | False |
| tlsv12 | <p>Whether to enable TLSv1.2.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | bool | no |  | False |
| tlsv13 | <p>Whether to enable TLSv1.3.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | bool | no |  | False |

### Options for nginx_http_config_files > server > auth_request_set
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| variable | <p>The variable to set.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_request_module.html#auth_request_set</p> | str | yes |  |  |
| value | <p>The value to set.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_request_module.html#auth_request_set</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > auth_jwt
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable JWT authentication.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt</p> | bool | no |  | False |
| realm | <p>The JWT authentication realm.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt</p> | str | no |  |  |
| token | <p>The JWT token.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt</p> | str | no |  |  |

### Options for nginx_http_config_files > server > auth_jwt_require
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| values | <p>The list of values for the claim.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_require</p> | list of 'str' | yes |  |  |
| error | <p>The error code to return if the claim is missing.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_require</p> | int | no | <ul><li>401</li><li>403</li></ul> |  |

### Options for nginx_http_config_files > server > gunzip_buffers
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| number | <p>The number of buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_gunzip_module.html#gunzip_buffers</p> | int | yes |  |  |
| size | <p>The size of the buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_gunzip_module.html#gunzip_buffers</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > gzip_buffers
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| number | <p>The number of buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_buffers</p> | int | yes |  |  |
| size | <p>The size of the buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_buffers</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > gzip_proxied
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable response compression for all proxies.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_proxied</p> | bool | no |  | False |
| any | <p>Whether to enable response compression for any proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_proxied</p> | bool | no |  | False |
| conditions | <p>The list of proxies for which to enable response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_proxied</p> | list of 'str' | no | <ul><li>expired</li><li>no-cache</li><li>no-store</li><li>private</li><li>no_last_modified</li><li>no_etag</li><li>auth</li></ul> |  |

### Options for nginx_http_config_files > server > add_header
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| field | <p>The header field to add.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_header</p> | str | yes |  |  |
| value | <p>The header value to add.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_header</p> | str | yes |  |  |
| always | <p>Whether to add the header to all responses.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_header</p> | bool | no |  | False |

### Options for nginx_http_config_files > server > add_trailer
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| field | <p>The trailer field to add.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_trailer</p> | str | yes |  |  |
| value | <p>The trailer value to add.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_trailer</p> | str | yes |  |  |
| always | <p>Whether to add the trailer to all responses.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_trailer</p> | bool | no |  | False |

### Options for nginx_http_config_files > server > expires
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable expiration.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | bool | no |  | False |
| max | <p>Whether to set the expiration to the maximum value.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | bool | no |  | False |
| epoch | <p>Whether to set the expiration to the minimum value and disable caching that way.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | bool | no |  | False |
| modified | <p>Whether to set the expiration relative to the last modification time.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | bool | no |  |  |
| time | <p>The time to set the expiration.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | str | no |  |  |

### Options for nginx_http_config_files > server > limit_req
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| zone | <p>The shared memory zone for rate limiting.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req</p> | str | yes |  |  |
| burst | <p>The maximum burst size.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req</p> | int | no |  |  |
| nodelay | <p>Whether to delay requests exceeding the rate limit.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req</p> | bool | no |  | False |
| delay | <p>The time period to delay requests exceeding the rate limit.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req</p> | str | no |  |  |

### Options for nginx_http_config_files > server > access_log
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable access logging.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | bool | no |  | False |
| logs | <p>The list of access log files.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | list of dicts of 'logs' options | no |  |  |

### Options for nginx_http_config_files > server > access_log > logs
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| path | <p>The path to the access log file.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | path | yes |  |  |
| format | <p>The log format.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |
| buffer | <p>The buffer size for the access log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |
| gzip | <p>The gzip configuration for the access log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | dict of 'gzip' options | no |  |  |
| flush | <p>The time period to flush the access log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |
| if | <p>The condition under which to log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |

### Options for nginx_http_config_files > server > access_log > logs > gzip
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| level | <p>The compression level.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | int | no |  |  |

### Options for nginx_http_config_files > server > open_log_file_cache
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the open log file cache.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#open_log_file_cache</p> | bool | no |  | False |
| max | <p>The maximum number of open log files to cache.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#open_log_file_cache</p> | int | no |  |  |
| inactive | <p>The time period to keep an open log file in the cache.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#open_log_file_cache</p> | str | no |  |  |
| min_uses | <p>The minimum number of uses for an open log file to keep it in the cache.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#open_log_file_cache</p> | int | no |  |  |
| valid | <p>The time period to consider a cached open log file valid.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#open_log_file_cache</p> | str | no |  |  |

### Options for nginx_http_config_files > server > error_log
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| file | <p>The path to the error log file.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#error_log</p> | path | no |  |  |
| level | <p>The log level for the error log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#error_log</p> | str | no | <ul><li>debug</li><li>info</li><li>notice</li><li>warn</li><li>error</li><li>crit</li><li>alert</li><li>emerg</li></ul> |  |

### Options for nginx_http_config_files > server > mirror
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable mirroring.</p><p>https://nginx.org/en/docs/http/ngx_http_mirror_module.html#mirror</p> | bool | no |  | False |
| uris | <p>The list of URIs to mirror.</p><p>https://nginx.org/en/docs/http/ngx_http_mirror_module.html#mirror</p> | list of 'str' | no |  |  |

### Options for nginx_http_config_files > server > return
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| code | <p>The status code to return.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#return</p> | int | no |  |  |
| url | <p>The URI to return.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#return</p> | str | no |  |  |
| text | <p>The text to return.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#return</p> | str | no |  |  |

### Options for nginx_http_config_files > server > rewrite
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| regex | <p>The regular expression to match.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite</p> | str | yes |  |  |
| replacement | <p>The replacement value.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite</p> | str | yes |  |  |
| flag | <p>The flags for the rewrite rule.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite</p> | str | no |  |  |

### Options for nginx_http_config_files > server > set
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| variable | <p>The variable to set.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#set</p> | str | yes |  |  |
| value | <p>The value to set.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#set</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > sub_filter
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| pattern | <p>The pattern to match.</p><p>https://nginx.org/en/docs/http/ngx_http_sub_module.html#sub_filter</p> | str | yes |  |  |
| replacement | <p>The replacement value.</p><p>https://nginx.org/en/docs/http/ngx_http_sub_module.html#sub_filter</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > location
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| modifier | <p>The modifier for the location block.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#location</p> | str | no | <ul><li>=</li><li>~</li><li>~*</li><li>^~</li></ul> |  |
| path | <p>The location path.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#location</p> | path | yes |  |  |
| error_page | <p>The error page configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#error_page</p> | list of dicts of 'error_page' options | no |  |  |
| limit_rate | <p>The rate limit for the response.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#limit_rate</p> | str | no |  |  |
| limit_rate_after | <p>The number of bytes after which to apply the rate limit.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#limit_rate_after</p> | str | no |  |  |
| root | <p>The root directory for the server.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#root</p> | path | no |  |  |
| sendfile | <p>Whether to use the sendfile system call.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#sendfile</p> | bool | no |  |  |
| absolute_redirect | <p>Whether to use absolute URIs in redirects.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#absolute_redirect</p> | bool | no |  |  |
| aio | <p>Whether to use asynchronous I/O.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#aio</p> | bool | no |  |  |
| auth_delay | <p>The delay for authentication.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#auth_delay</p> | str | no |  |  |
| chunked_transfer_encoding | <p>Whether to use chunked transfer encoding.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#chunked_transfer_encoding</p> | bool | no |  |  |
| client_body_buffer_size | <p>The size of the client request body buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_buffer_size</p> | str | no |  |  |
| client_body_in_file_only | <p>Whether to store the client request body in a file.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_in_file_only</p> | str | no | <ul><li>on</li><li>clean</li><li>off</li></ul> |  |
| client_body_in_single_buffer | <p>Whether to store the client request body in a single buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_in_single_buffer</p> | bool | no |  |  |
| client_body_temp_path | <p>The client request body temporary storage configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_temp_path</p> | dict of 'client_body_temp_path' options | no |  |  |
| client_body_timeout | <p>The timeout for reading the client request body.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_timeout</p> | str | no |  |  |
| client_max_body_size | <p>The maximum size of the client request body.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_max_body_size</p> | str | no |  |  |
| default_type | <p>The default MIME type.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#default_type</p> | str | no |  |  |
| directio | <p>If set to `off`, disables the use of direct I/O.</p><p>If set to a size, enables the use of direct I/O with the specified size.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#directio</p> | str | no |  |  |
| directio_alignment | <p>The alignment for direct I/O.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#directio_alignment</p> | str | no |  |  |
| disable_symlinks | <p>The symbolic link configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#disable_symlinks</p> | dict of 'disable_symlinks' options | no |  |  |
| etag | <p>Whether to enable ETag generation.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#etag</p> | bool | no |  |  |
| if_modified_since | <p>The If-Modified-Since header configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#if_modified_since</p> | str | no | <ul><li>off</li><li>exact</li><li>before</li></ul> |  |
| keepalive_disable | <p>The keepalive disable configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_disable</p> | dict of 'keepalive_disable' options | no |  |  |
| keepalive_requests | <p>The maximum number of requests to allow on a keepalive connection.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_requests</p> | int | no |  |  |
| keepalive_time | <p>The time period over which to keep a keepalive connection open.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_time</p> | str | no |  |  |
| keepalive_timeout | <p>The timeout for keepalive connections.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_timeout</p> | dict of 'keepalive_timeout' options | no |  |  |
| lingering_close | <p>The lingering close configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#lingering_close</p> | str | no | <ul><li>off</li><li>on</li><li>always</li></ul> |  |
| lingering_time | <p>The time period over which to wait for more data.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#lingering_time</p> | str | no |  |  |
| lingering_timeout | <p>The timeout for lingering connections.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#lingering_timeout</p> | str | no |  |  |
| log_not_found | <p>Whether to log missing files.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#log_not_found</p> | bool | no |  |  |
| log_subrequest | <p>Whether to log subrequests.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#log_subrequest</p> | bool | no |  |  |
| max_ranges | <p>The maximum number of ranges to allow in a request.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#max_ranges</p> | int | no |  |  |
| msie_padding | <p>Whether to enable MSIE padding.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#msie_padding</p> | bool | no |  |  |
| msie_refresh | <p>Whether to enable MSIE refresh.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#msie_refresh</p> | bool | no |  |  |
| open_file_cache | <p>The open file cache configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache</p> | dict of 'open_file_cache' options | no |  |  |
| open_file_cache_errors | <p>Whether to cache errors.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache_errors</p> | bool | no |  |  |
| open_file_cache_min_uses | <p>The minimum number of uses to cache a file.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache_min_uses</p> | int | no |  |  |
| open_file_cache_valid | <p>The time period for which to cache a file.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache_valid</p> | str | no |  |  |
| output_buffers | <p>The output buffer configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#output_buffers</p> | dict of 'output_buffers' options | no |  |  |
| port_in_redirect | <p>Whether to include the port in redirects.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#port_in_redirect</p> | bool | no |  |  |
| postpone_output | <p>The number of bytes NGINX will wait for before sending data, if possible.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#postpone_output</p> | int | no |  |  |
| read_ahead | <p>The read ahead size configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#read_ahead</p> | str | no |  |  |
| recursive_error_pages | <p>Whether to enable recursive error pages.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#recursive_error_pages</p> | bool | no |  |  |
| reset_timedout_connection | <p>Whether to reset timed out connections.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#reset_timedout_connection</p> | bool | no |  |  |
| resolver | <p>The list of resolver configurations.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver</p> | list of dicts of 'resolver' options | no |  | [] |
| resolver_timeout | <p>The timeout for resolver queries.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver_timeout</p> | str | no |  |  |
| satisfy | <p>The satisfy configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#satisfy</p> | str | no | <ul><li>all</li><li>any</li></ul> |  |
| send_lowat | <p>The low water mark for sending data.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#send_lowat</p> | str | no |  |  |
| send_timeout | <p>The timeout for sending data.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#send_timeout</p> | str | no |  |  |
| sendfile_max_chunk | <p>The maximum size of a file chunk to send.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#sendfile_max_chunk</p> | str | no |  |  |
| server_name_in_redirect | <p>Whether to include the server name in redirects.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#server_name_in_redirect</p> | bool | no |  |  |
| server_tokens | <p>The server tokens configuration.</p><p>If set to `on`, the server version will be included in error messages.</p><p>If set to `off`, the server version will not be included in error messages.</p><p>If set to `build`, the server version will be included in error messages, but the NGINX version will not be included.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#server_tokens</p> | str | no | <ul><li>on</li><li>off</li><li>build</li></ul> |  |
| subrequest_output_buffer_size | <p>The size of the subrequest output buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#subrequest_output_buffer_size</p> | str | no |  |  |
| tcp_nodelay | <p>Whether to enable TCP_NODELAY.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#tcp_nodelay</p> | bool | no |  |  |
| tcp_nopush | <p>Whether to enable TCP_NOPUSH.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#tcp_nopush</p> | bool | no |  |  |
| types_hash_bucket_size | <p>The size of the types hash bucket.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#types_hash_bucket_size</p> | int | no |  |  |
| types_hash_max_size | <p>The maximum size of the types hash.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#types_hash_max_size</p> | int | no |  |  |
| try_files | <p>The list of files to try.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#try_files</p> | list of dicts of 'try_files' options | no |  | [] |
| alias | <p>The alias for the location.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#alias</p> | str | no |  |  |
| internal | <p>Whether the location is internal.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#internal</p> | bool | no |  | False |
| http2_chunk_size | <p>The size of the HTTP/2 chunk.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_chunk_size</p> | str | no |  |  |
| http2_push | <p>The HTTP/2 push configuration.</p><p>If set to `off`, disables HTTP/2 push.</p><p>If set to a URI, enables HTTP/2 push for the specified URI.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_push</p> | str | no |  |  |
| http2_push_preload | <p>Whether to enable HTTP/2 push preload.</p><p>https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2_push_preload</p> | bool | no |  |  |
| proxy_bind | <p>The proxy binding configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_bind</p> | dict of 'proxy_bind' options | no |  |  |
| proxy_buffer_size | <p>The size of the proxy buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffer_size</p> | str | no |  |  |
| proxy_buffers | <p>The proxy buffers configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffers</p> | dict of 'proxy_buffers' options | no |  |  |
| proxy_busy_buffers_size | <p>The size of the proxy busy buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_busy_buffers_size</p> | str | no |  |  |
| proxy_cache | <p>The proxy cache zone configuration.</p><p>If set to `off`, disables the proxy cache.</p><p>If set to a string, enables the proxy cache with the specified zone name.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache</p> | str | no |  |  |
| proxy_cache_background_update | <p>Whether to enable background updates for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_background_update</p> | bool | no |  |  |
| proxy_cache_bypass | <p>The list of proxy cache bypass rules.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_bypass</p> | list of 'str' | no |  | [] |
| proxy_cache_convert_head | <p>Whether to convert HEAD requests to GET requests for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_convert_head</p> | bool | no |  |  |
| proxy_cache_key | <p>The proxy cache key configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_key</p> | str | no |  |  |
| proxy_cache_lock | <p>Whether to enable locking for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_lock</p> | bool | no |  |  |
| proxy_cache_lock_age | <p>The time period after which to consider a lock stale.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_lock_age</p> | str | no |  |  |
| proxy_cache_lock_timeout | <p>The timeout for acquiring a lock for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_lock_timeout</p> | str | no |  |  |
| proxy_cache_max_range_offset | <p>The maximum range offset for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_max_range_offset</p> | str | no |  |  |
| proxy_cache_methods | <p>The list of methods to cache for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_methods</p> | list of 'str' | no | <ul><li>GET</li><li>HEAD</li><li>POST</li></ul> | [] |
| proxy_cache_min_uses | <p>The minimum number of uses to cache a response for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_min_uses</p> | int | no |  |  |
| proxy_cache_purge | <p>A list of conditions under which to purge the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_purge</p> | list of 'str' | no |  | [] |
| proxy_cache_revalidate | <p>Whether to enable revalidation for the proxy cache.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_revalidate</p> | bool | no |  |  |
| proxy_cache_use_stale | <p>The configuration for the proxy_cache_use_stale directive.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_use_stale</p> | list of dicts of 'proxy_cache_use_stale' options | no |  |  |
| proxy_cache_valid | <p>The proxy cache validity configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_valid</p> | list of dicts of 'proxy_cache_valid' options | no |  | [] |
| proxy_connect_timeout | <p>The timeout for connecting to the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_connect_timeout</p> | str | no |  |  |
| proxy_cookie_domain | <p>The proxy cookie domain configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_domain</p> | dict of 'proxy_cookie_domain' options | no |  |  |
| proxy_cookie_flags | <p>The proxy cookie flags configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_flags</p> | dict of 'proxy_cookie_flags' options | no |  |  |
| proxy_cookie_path | <p>The proxy cookie path configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_path</p> | dict of 'proxy_cookie_path' options | no |  |  |
| proxy_force_ranges | <p>Whether to force byte ranges for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_force_ranges</p> | bool | no |  |  |
| proxy_headers_hash_bucket_size | <p>The size of the proxy headers hash bucket.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_headers_hash_bucket_size</p> | int | no |  |  |
| proxy_headers_hash_max_size | <p>The maximum size of the proxy headers hash.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_headers_hash_max_size</p> | int | no |  |  |
| proxy_hide_header | <p>The list of headers to hide from the proxy response.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_hide_header</p> | list of 'str' | no |  | [] |
| proxy_http_version | <p>The HTTP version to use for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_http_version</p> | str | no | <ul><li>1.0</li><li>1.1</li></ul> |  |
| proxy_ignore_client_abort | <p>Whether to ignore client aborts for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ignore_client_abort</p> | bool | no |  |  |
| proxy_ignore_headers | <p>The list of headers to ignore from the proxy response.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ignore_headers</p> | list of 'str' | no | <ul><li>X-Accel-Redirect</li><li>X-Accel-Expires</li><li>X-Accel-Limit-Rate</li><li>X-Accel-Buffering</li><li>X-Accel-Charset</li><li>Expires</li><li>Cache-Control</li><li>Set-Cookie</li><li>Vary</li></ul> | [] |
| proxy_intercept_errors | <p>Whether to intercept errors for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_intercept_errors</p> | bool | no |  |  |
| proxy_limit_rate | <p>The rate limit for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_limit_rate</p> | str | no |  |  |
| proxy_max_temp_file_size | <p>The maximum size of a temporary file for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_max_temp_file_size</p> | str | no |  |  |
| proxy_method | <p>The method to use for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_method</p> | str | no |  |  |
| proxy_next_upstream | <p>The configuration for the proxy_next_upstream directive.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream</p> | list of dicts of 'proxy_next_upstream' options | no |  | [] |
| proxy_next_upstream_timeout | <p>The timeout for selecting the next upstream server for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream_timeout</p> | str | no |  |  |
| proxy_next_upstream_tries | <p>The number of tries to select the next upstream server for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream_tries</p> | int | no |  |  |
| proxy_no_cache | <p>The list of conditions under which to disable caching for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_no_cache</p> | list of 'str' | no |  | [] |
| proxy_pass_header | <p>The list of headers to pass to the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass_header</p> | list of 'str' | no |  | [] |
| proxy_pass_request_body | <p>Whether to pass the request</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass_request_body</p> | bool | no |  |  |
| proxy_pass_request_headers | <p>Whether to pass the request</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass_request_headers</p> | bool | no |  |  |
| proxy_read_timeout | <p>The timeout for reading from the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_read_timeout</p> | str | no |  |  |
| proxy_redirect | <p>The proxy redirect configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_redirect</p> | dict of 'proxy_redirect' options | no |  |  |
| proxy_request_buffering | <p>Whether to buffer the request for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_request_buffering</p> | bool | no |  |  |
| proxy_send_lowat | <p>The low water mark for sending data to the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_send_lowat</p> | str | no |  |  |
| proxy_send_timeout | <p>The timeout for sending data to the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_send_timeout</p> | str | no |  |  |
| proxy_set_body | <p>The list of body replacements for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_set_body</p> | str | no |  |  |
| proxy_set_header | <p>The list of headers to set for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_set_header</p> | list of dicts of 'proxy_set_header' options | no |  | [] |
| proxy_socket_keepalive | <p>Whether to enable socket keepalive for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_socket_keepalive</p> | bool | no |  |  |
| proxy_ssl_certificate | <p>The path to the SSL certificate for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_certificate</p> | path | no |  |  |
| proxy_ssl_certificate_key | <p>The path to the SSL certificate key for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_certificate_key</p> | path | no |  |  |
| proxy_ssl_ciphers | <p>The SSL ciphers configuration in OpenSSL format for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_ciphers</p> | str | no |  |  |
| proxy_ssl_conf_command | <p>The list of SSL configuration commands for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_conf_command</p> | list of 'str' | no |  | [] |
| proxy_ssl_crl | <p>The path to the SSL certificate revocation list for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_crl</p> | path | no |  |  |
| proxy_ssl_name | <p>The name to send for the SNI for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_name</p> | str | no |  |  |
| proxy_ssl_password_file | <p>The path to the SSL password file for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_password_file</p> | path | no |  |  |
| proxy_ssl_protocols | <p>The SSL protocols configuration for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | dict of 'proxy_ssl_protocols' options | no |  |  |
| proxy_ssl_server_name | <p>Whether to enable TLS Server Name Indication (SNI) for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_server_name</p> | bool | no |  |  |
| proxy_ssl_session_reuse | <p>Whether to enable SSL session reuse for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_session_reuse</p> | bool | no |  |  |
| proxy_ssl_trusted_certificate | <p>The path to the SSL trusted certificate for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_trusted_certificate</p> | path | no |  |  |
| proxy_ssl_verify | <p>Whether to enable SSL verification for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_verify</p> | bool | no |  |  |
| proxy_ssl_verify_depth | <p>The maximum depth of the SSL certificate verification chain for the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_verify_depth</p> | int | no |  |  |
| proxy_store | <p>The path to store the proxy response.</p><p>If set to `off`, disables storing the proxy response.</p><p>If set to `on`, stores the proxy response in the default location.</p><p>If set to a path, stores the proxy response in the specified location.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_store</p> | str | no |  |  |
| proxy_store_access | <p>The file permissions for the stored proxy responses.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_store_access</p> | dict of 'proxy_store_access' options | no |  |  |
| proxy_temp_file_write_size | <p>The size of the temporary file for writing to the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_temp_file_write_size</p> | str | no |  |  |
| proxy_temp_path | <p>The proxy temporary path configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_temp_path</p> | dict of 'proxy_temp_path' options | no |  |  |
| proxy_pass | <p>The URI to which to pass the request.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass</p> | str | no |  |  |
| grpc_bind | <p>The gRPC server bind configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_bind</p> | dict of 'grpc_bind' options | no |  |  |
| grpc_buffer_size | <p>The size of the gRPC buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_buffer_size</p> | str | no |  |  |
| grpc_connect_timeout | <p>The timeout for connecting to the gRPC server.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_connect_timeout</p> | str | no |  |  |
| grpc_hide_header | <p>The list of headers to hide from the gRPC response.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_hide_header</p> | list of 'str' | no |  | [] |
| grpc_ignore_headers | <p>The list of headers to ignore from the gRPC response.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ignore_headers</p> | list of 'str' | no | <ul><li>X-Accel-Redirect</li><li>X-Accel-Charset</li></ul> | [] |
| grpc_intercept_errors | <p>Whether to intercept errors for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_intercept_errors</p> | bool | no |  |  |
| grpc_next_upstream | <p>The list of conditions under which to select the next upstream server for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_next_upstream</p> | dict of 'grpc_next_upstream' options | no |  |  |
| grpc_next_upstream_timeout | <p>The timeout for selecting the next upstream server for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_next_upstream_timeout</p> | str | no |  |  |
| grpc_next_upstream_tries | <p>The number of tries to select the next upstream server for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_next_upstream_tries</p> | int | no |  |  |
| grpc_pass_header | <p>The list of headers to pass to the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_pass_header</p> | list of 'str' | no |  | [] |
| grpc_read_timeout | <p>The timeout for reading from the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_read_timeout</p> | str | no |  |  |
| grpc_send_timeout | <p>The timeout for sending data to the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_send_timeout</p> | str | no |  |  |
| grpc_set_header | <p>The list of headers to set for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_set_header</p> | list of dicts of 'grpc_set_header' options | no |  | [] |
| grpc_socket_keepalive | <p>Whether to enable socket keepalive for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_socket_keepalive</p> | bool | no |  |  |
| grpc_ssl_certificate | <p>The path to the SSL certificate for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_certificate</p> | path | no |  |  |
| grpc_ssl_certificate_key | <p>The path to the SSL certificate key for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_certificate_key</p> | path | no |  |  |
| grpc_ssl_ciphers | <p>The SSL ciphers configuration in OpenSSL format for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_ciphers</p> | str | no |  |  |
| grpc_ssl_conf_command | <p>The list of SSL configuration commands for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_conf_command</p> | list of 'str' | no |  | [] |
| grpc_ssl_crl | <p>The path to the SSL certificate revocation list for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_crl</p> | path | no |  |  |
| grpc_ssl_name | <p>The name to send for the SNI for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_name</p> | str | no |  |  |
| grpc_ssl_password_file | <p>The path to the SSL password file for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_password_file</p> | path | no |  |  |
| grpc_ssl_protocols | <p>The SSL protocols configuration for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | dict of 'grpc_ssl_protocols' options | no |  |  |
| grpc_ssl_server_name | <p>Whether to enable TLS Server Name Indication (SNI) for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_server_name</p> | bool | no |  |  |
| grpc_ssl_session_reuse | <p>Whether to enable SSL session reuse for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_session_reuse</p> | bool | no |  |  |
| grpc_ssl_trusted_certificate | <p>The path to the SSL trusted certificate for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_trusted_certificate</p> | path | no |  |  |
| grpc_ssl_verify | <p>Whether to enable SSL verification for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_verify</p> | bool | no |  |  |
| grpc_ssl_verify_depth | <p>The maximum depth of the SSL certificate verification chain for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_verify_depth</p> | int | no |  |  |
| grpc_pass | <p>The URI to which to pass the request.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_pass</p> | str | no |  |  |
| allow | <p>The list of IP addresses or CIDR blocks to allow.</p><p>https://nginx.org/en/docs/http/ngx_http_access_module.html#allow</p> | list of 'str' | no |  | [] |
| deny | <p>The list of IP addresses or CIDR blocks to deny.</p><p>https://nginx.org/en/docs/http/ngx_http_access_module.html#deny</p> | list of 'str' | no |  | [] |
| auth_basic | <p>The basic authentication realm.</p><p>If set to `off`, disables basic authentication.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_basic_module.html#auth_basic</p> | str | no |  |  |
| auth_basic_user_file | <p>The path to the basic authentication user file.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_basic_module.html#auth_basic_user_file</p> | path | no |  |  |
| auth_request | <p>The URI to request for authentication.</p><p>If set to `off`, disables authentication requests.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_request_module.html#auth_request</p> | str | no |  |  |
| auth_request_set | <p>The list of variables to set from the authentication request.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_request_module.html#auth_request_set</p> | list of dicts of 'auth_request_set' options | no |  | [] |
| auth_jwt | <p>The JWT authentication configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt</p> | dict of 'auth_jwt' options | no |  |  |
| auth_jwt_key_file | <p>The path to the JWT key file.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_key_file</p> | path | no |  |  |
| auth_jwt_key_request | <p>The URI to request the JWT key.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_key_request</p> | str | no |  |  |
| auth_jwt_type | <p>The JWT type.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_type</p> | str | no | <ul><li>signed</li><li>encrypted</li><li>nested</li></ul> |  |
| auth_jwt_require | <p>The list of claims required in the JWT.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_require</p> | list of dicts of 'auth_jwt_require' options | no |  | [] |
| auth_jwt_key_cache | <p>The time to cache JWT keys.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_key_cache</p> | str | no |  |  |
| auth_jwt_leeway | <p>The leeway time for JWT verification.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_leeway</p> | str | no |  |  |
| stub_status | <p>Whether to enable the stub status module.</p><p>https://nginx.org/en/docs/http/ngx_http_stub_status_module.html#stub_status</p> | bool | no |  | False |
| autoindex | <p>Whether to enable directory listing.</p><p>https://nginx.org/en/docs/http/ngx_http_autoindex_module.html#autoindex</p> | bool | no |  |  |
| autoindex_exact_size | <p>Whether to display the exact file size in directory listings.</p><p>https://nginx.org/en/docs/http/ngx_http_autoindex_module.html#autoindex_exact_size</p> | bool | no |  |  |
| autoindex_format | <p>The format of the directory listing.</p><p>https://nginx.org/en/docs/http/ngx_http_autoindex_module.html#autoindex_format</p> | str | no | <ul><li>html</li><li>xml</li><li>json</li><li>jsonp</li></ul> |  |
| autoindex_localtime | <p>Whether to display the file modification time in local time in directory listings.</p><p>https://nginx.org/en/docs/http/ngx_http_autoindex_module.html#autoindex_localtime</p> | bool | no |  |  |
| index | <p>The list of files to use as the index.</p><p>https://nginx.org/en/docs/http/ngx_http_index_module.html#index</p> | list of 'str' | no |  |  |
| random_index | <p>Whether to select a random file from the index.</p><p>https://nginx.org/en/docs/http/ngx_http_random_index_module.html#random_index</p> | bool | no |  |  |
| gunzip | <p>Whether to enable response decompression.</p><p>https://nginx.org/en/docs/http/ngx_http_gunzip_module.html#gunzip</p> | bool | no |  |  |
| gunzip_buffers | <p>The size and number of buffers for response decompression.</p><p>https://nginx.org/en/docs/http/ngx_http_gunzip_module.html#gunzip_buffers</p> | dict of 'gunzip_buffers' options | no |  |  |
| gzip | <p>Whether to enable response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip</p> | bool | no |  |  |
| gzip_buffers | <p>The size and number of buffers for response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_buffers</p> | dict of 'gzip_buffers' options | no |  |  |
| gzip_disable | <p>The list of user agents to disable response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_disable</p> | list of 'str' | no |  | [] |
| gzip_http_version | <p>The HTTP version for response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_http_version</p> | str | no | <ul><li>1.0</li><li>1.1</li></ul> |  |
| gzip_min_length | <p>The minimum length of the response to compress.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_min_length</p> | str | no |  |  |
| gzip_proxied | <p>The list of proxies for which to enable response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_proxied</p> | dict of 'gzip_proxied' options | no |  |  |
| gzip_types | <p>The list of MIME types to compress.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_types</p> | list of 'str' | no |  | [] |
| gzip_vary | <p>Whether to add the Vary header for response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_vary</p> | bool | no |  |  |
| gzip_static | <p>Whether to use precompressed files for response compression.</p><p>If set to `on`, uses precompressed files.</p><p>If set to `always`, uses precompressed files without checking client support.</p><p>If set to `off`, does not use precompressed files.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_static_module.html#gzip_static</p> | str | no | <ul><li>on</li><li>off</li><li>always</li></ul> |  |
| add_header | <p>The list of headers to add to the response.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_header</p> | list of dicts of 'add_header' options | no |  | [] |
| add_trailer | <p>The list of trailers to add to the response.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_trailer</p> | list of dicts of 'add_trailer' options | no |  | [] |
| expires | <p>The expiration configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | dict of 'expires' options | no |  |  |
| limit_req | <p>The request rate limiting configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req</p> | list of dicts of 'limit_req' options | no |  |  |
| limit_req_dry_run | <p>Whether to enable dry-run mode for rate limiting.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req_dry_run</p> | bool | no |  |  |
| limit_req_log_level | <p>The log level for rate limiting.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req_log_level</p> | str | no | <ul><li>info</li><li>notice</li><li>warn</li><li>error</li></ul> |  |
| limit_req_status | <p>The status code to return for requests exceeding the rate limit.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req_status</p> | int | no |  |  |
| access_log | <p>The access log configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | dict of 'access_log' options | no |  |  |
| open_log_file_cache | <p>The open log file cache configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#open_log_file_cache</p> | dict of 'open_log_file_cache' options | no |  |  |
| error_log | <p>The error log configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#error_log</p> | dict of 'error_log' options | no |  |  |
| mirror | <p>The mirror configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_mirror_module.html#mirror</p> | dict of 'mirror' options | no |  |  |
| mirror_request_body | <p>Whether to mirror the request body.</p><p>https://nginx.org/en/docs/http/ngx_http_mirror_module.html#mirror_request_body</p> | bool | no |  |  |
| set_real_ip_from | <p>The IP address or CIDR block from which to set the real IP.</p><p>https://nginx.org/en/docs/http/ngx_http_realip_module.html#set_real_ip_from</p> | list of 'str' | no |  | [] |
| real_ip_header | <p>The header from which to set the real IP.</p><p>https://nginx.org/en/docs/http/ngx_http_realip_module.html#real_ip_header</p> | str | no |  |  |
| real_ip_recursive | <p>Whether to set the real IP recursively.</p><p>https://nginx.org/en/docs/http/ngx_http_realip_module.html#real_ip_recursive</p> | bool | no |  |  |
| rewrite_log | <p>Whether to enable rewrite logging.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite_log</p> | bool | no |  |  |
| uninitialized_variable_warn | <p>Whether to warn about uninitialized variables.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#uninitialized_variable_warn</p> | bool | no |  |  |
| break | <p>Whether to break rewrite processing.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#break</p> | bool | no |  |  |
| return | <p>The return configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#return</p> | dict of 'return' options | no |  |  |
| rewrite | <p>The list of rewrite rules.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite</p> | list of dicts of 'rewrite' options | no |  | [] |
| set | <p>The list of variables to set.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#set</p> | list of dicts of 'set' options | no |  | [] |
| sub_filter | <p>The list of substitutions to make in the response body.</p><p>https://nginx.org/en/docs/http/ngx_http_sub_module.html#sub_filter</p> | list of dicts of 'sub_filter' options | no |  | [] |
| sub_filter_last_modified | <p>Whether to replace the last modified time in the response body.</p><p>https://nginx.org/en/docs/http/ngx_http_sub_module.html#sub_filter_last_modified</p> | bool | no |  |  |
| sub_filter_once | <p>Whether to replace only the first match in the response body.</p><p>https://nginx.org/en/docs/http/ngx_http_sub_module.html#sub_filter_once</p> | bool | no |  |  |
| sub_filter_types | <p>The list of MIME types to replace in the response body.</p><p>https://nginx.org/en/docs/http/ngx_http_sub_module.html#sub_filter_types</p> | list of 'str' | no |  | [] |
| custom_directives | <p>The list of custom directives to include in the configuration.</p> | list of 'str' | no |  | [] |
| location | <p>The list of location blocks.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#location</p> | list of 'dict' | no |  | [] |
| if | <p>The list of if blocks.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#if</p> | list of dicts of 'if' options | no |  | [] |
| limit_except | <p>The list of HTTP methods to limit.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#limit_except</p> | list of dicts of 'limit_except' options | no |  | [] |

### Options for nginx_http_config_files > server > location > error_page
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| codes | <p>The list of error codes for which this directive applies.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#error_page</p> | list of 'int' | yes |  |  |
| response | <p>The response code configuration when the specified error codes would normally occur.</p><p>If not defined, the response code from the redirected URI is used.</p><p>If defined, this must be either `=` or `=XXX` with XXX being a response code.</p><p>If set to `=`, the original response code is retained.</p><p>If set to `=XXX`, the response code is replaced with `XXX`.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#error_page</p> | str | no |  |  |
| uri | <p>The URI of the error page.</p><p>Alternatively, this can be set to `@` followed by the name of a location block to reference it.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#error_page</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > location > client_body_temp_path
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| path | <p>The path to the temporary storage directory.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_temp_path</p> | path | yes |  |  |
| level | <p>The number of subdirectories in the temporary storage directory.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#client_body_temp_path</p> | int | no | <ul><li>0</li><li>1</li><li>2</li><li>3</li></ul> | 0 |

### Options for nginx_http_config_files > server > location > disable_symlinks
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable symbolic links.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#disable_symlinks</p> | bool | no |  | False |
| if_not_owner | <p>Whether to disable symbolic links if the owner of the file is not the same as the owner of the link.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#disable_symlinks</p> | bool | no |  | False |
| from | <p>The source of the symbolic link.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#disable_symlinks</p> | str | no |  |  |

### Options for nginx_http_config_files > server > location > keepalive_disable
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| none | <p>Whether to disable keepalive connections.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_disable</p> | bool | no |  | False |
| browsers | <p>The list of browsers for which to disable keepalive connections.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_disable</p> | list of 'str' | no |  | [] |

### Options for nginx_http_config_files > server > location > keepalive_timeout
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| timeout | <p>The time period for the keepalive timeout.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_timeout</p> | str | yes |  |  |
| header_timeout | <p>The time period for the keepalive header timeout.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_timeout</p> | str | no |  |  |

### Options for nginx_http_config_files > server > location > open_file_cache
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the open file cache.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache</p> | bool | no |  | False |
| max | <p>The maximum number of files to cache.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache</p> | int | no |  |  |
| inactive | <p>The time period after which to remove a file from the cache.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#open_file_cache</p> | str | no |  |  |

### Options for nginx_http_config_files > server > location > output_buffers
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| count | <p>The number of buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#output_buffers</p> | int | yes |  |  |
| size | <p>The size of the buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#output_buffers</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > location > resolver
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| addresses | <p>The address of the resolver.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver</p> | list of 'str' | yes |  |  |
| valid | <p>The time period for which to cache a response.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver</p> | str | no |  |  |
| ipv4 | <p>Whether to resolve IPv4 addresses.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver</p> | bool | no |  | False |
| ipv6 | <p>Whether to resolve IPv6 addresses.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver</p> | bool | no |  | False |
| status_zone | <p>The shared memory zone for resolver status.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver</p> | str | no |  |  |

### Options for nginx_http_config_files > server > location > try_files
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| files | <p>The list of files to try.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#try_files</p> | list of 'path' | yes |  |  |
| response | <p>The response to send if the file is not found.</p><p>This can either be a URI or an error code.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#try_files</p> | str | no |  |  |

### Options for nginx_http_config_files > server > location > proxy_bind
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable proxy binding.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_bind</p> | bool | no |  | False |
| address | <p>The address to which to bind the proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_bind</p> | str | no |  |  |
| transparent | <p>Whether to enable transparent proxy binding.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_bind</p> | bool | no |  | False |

### Options for nginx_http_config_files > server > location > proxy_buffers
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| count | <p>The number of buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffers</p> | int | yes |  |  |
| size | <p>The size of the buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffers</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > location > proxy_cache_use_stale
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the proxy_cache_use_stale directive.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_use_stale</p> | bool | no |  | False |
| conditions | <p>The list of conditions under which to use stale cache data.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_use_stale</p> | list of 'str' | no | <ul><li>error</li><li>timeout</li><li>invalid_header</li><li>updating</li><li>http_500</li><li>http_502</li><li>http_503</li><li>http_504</li><li>http_403</li><li>http_404</li><li>http_429</li></ul> |  |

### Options for nginx_http_config_files > server > location > proxy_cache_valid
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| codes | <p>The status codes for which to cache a response.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_valid</p> | list of 'int' | yes |  |  |
| time | <p>The time period for which to cache a response.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_valid</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > location > proxy_cookie_domain
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the proxy cookie domain.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_domain</p> | bool | no |  | False |
| domains | <p>The list of domain replacements.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_domain</p> | list of dicts of 'domains' options | no |  |  |

### Options for nginx_http_config_files > server > location > proxy_cookie_domain > domains
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| domain | <p>The domain to map.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_domain</p> | str | yes |  |  |
| replacement | <p>The domain replacement.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_domain</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > location > proxy_cookie_flags
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the proxy cookie flags.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_flags</p> | bool | no |  | False |
| flags | <p>The list of cookie flags.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_flags</p> | list of dicts of 'flags' options | no |  |  |

### Options for nginx_http_config_files > server > location > proxy_cookie_flags > flags
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| cookie | <p>The name of the cookie.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_flags</p> | str | yes |  |  |
| flags | <p>The flags for the cookie.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_flags</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > location > proxy_cookie_path
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the proxy cookie path.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_path</p> | bool | no |  | False |
| paths | <p>The list of path replacements.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_path</p> | list of dicts of 'paths' options | no |  |  |

### Options for nginx_http_config_files > server > location > proxy_cookie_path > paths
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| path | <p>The path to map.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_path</p> | str | yes |  |  |
| replacement | <p>The path replacement.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_path</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > location > proxy_next_upstream
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the proxy_next_upstream directive.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream</p> | bool | no |  | False |
| conditions | <p>The list of conditions under which to select the next upstream server.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream</p> | list of 'str' | no | <ul><li>error</li><li>timeout</li><li>invalid_header</li><li>http_500</li><li>http_502</li><li>http_503</li><li>http_504</li><li>http_403</li><li>http_404</li><li>http_429</li><li>non_idempotent</li></ul> |  |

### Options for nginx_http_config_files > server > location > proxy_redirect
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable proxy redirects.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_redirect</p> | bool | no |  | False |
| default | <p>Whether to enable the default proxy redirect.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_redirect</p> | bool | no |  |  |
| redirects | <p>The list of redirect rules.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_redirect</p> | list of dicts of 'redirects' options | no |  |  |

### Options for nginx_http_config_files > server > location > proxy_redirect > redirects
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| original | <p>The redirect original.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_redirect</p> | str | yes |  |  |
| replacement | <p>The redirect replacement.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_redirect</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > location > proxy_set_header
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| field | <p>The header field to set.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_set_header</p> | str | yes |  |  |
| value | <p>The header value to set.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_set_header</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > location > proxy_ssl_protocols
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| sslv2 | <p>Whether to enable SSLv2.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | bool | no |  | False |
| sslv3 | <p>Whether to enable SSLv3.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | bool | no |  | False |
| tlsv1 | <p>Whether to enable TLSv1.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | bool | no |  | False |
| tlsv11 | <p>Whether to enable TLSv1.1.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | bool | no |  | False |
| tlsv12 | <p>Whether to enable TLSv1.2.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | bool | no |  | False |
| tlsv13 | <p>Whether to enable TLSv1.3.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_protocols</p> | bool | no |  | False |

### Options for nginx_http_config_files > server > location > proxy_store_access
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| user | <p>The owner permissions in 'rwx' format for newly created files and directories.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_store_access</p> | str | yes |  |  |
| group | <p>The group permissions in 'rwx' format for newly created files and directories.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_store_access</p> | str | yes |  |  |
| all | <p>The general permissions in 'rwx' format for newly created files and directories.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_store_access</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > location > proxy_temp_path
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| path | <p>The path to the temporary directory.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_temp_path</p> | path | yes |  |  |
| level | <p>The number of levels of subdirectories to create.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_temp_path</p> | int | no | <ul><li>0</li><li>1</li><li>2</li><li>3</li></ul> | 0 |

### Options for nginx_http_config_files > server > location > grpc_bind
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the gRPC server bind.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_bind</p> | bool | no |  | False |
| address | <p>The address to bind the gRPC server.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_bind</p> | str | yes |  |  |
| transparent | <p>Whether to enable transparent proxying.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_bind</p> | bool | no |  | False |

### Options for nginx_http_config_files > server > location > grpc_next_upstream
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the gRPC next upstream.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_next_upstream</p> | bool | no |  | False |
| upstreams | <p>The list of conditions under which to select the next upstream server for the gRPC.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_next_upstream</p> | list of 'str' | no | <ul><li>error</li><li>timeout</li><li>invalid_header</li><li>http_500</li><li>http_502</li><li>http_503</li><li>http_504</li><li>http_403</li><li>http_404</li><li>http_429</li><li>non_idempotent</li></ul> |  |

### Options for nginx_http_config_files > server > location > grpc_set_header
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| field | <p>The header field to set.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_set_header</p> | str | yes |  |  |
| value | <p>The header value to set.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_set_header</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > location > grpc_ssl_protocols
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| sslv2 | <p>Whether to enable SSLv2.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | bool | no |  | False |
| sslv3 | <p>Whether to enable SSLv3.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | bool | no |  | False |
| tlsv1 | <p>Whether to enable TLSv1.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | bool | no |  | False |
| tlsv11 | <p>Whether to enable TLSv1.1.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | bool | no |  | False |
| tlsv12 | <p>Whether to enable TLSv1.2.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | bool | no |  | False |
| tlsv13 | <p>Whether to enable TLSv1.3.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_ssl_protocols</p> | bool | no |  | False |

### Options for nginx_http_config_files > server > location > auth_request_set
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| variable | <p>The variable to set.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_request_module.html#auth_request_set</p> | str | yes |  |  |
| value | <p>The value to set.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_request_module.html#auth_request_set</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > location > auth_jwt
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable JWT authentication.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt</p> | bool | no |  | False |
| realm | <p>The JWT authentication realm.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt</p> | str | no |  |  |
| token | <p>The JWT token.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt</p> | str | no |  |  |

### Options for nginx_http_config_files > server > location > auth_jwt_require
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| values | <p>The list of values for the claim.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_require</p> | list of 'str' | yes |  |  |
| error | <p>The error code to return if the claim is missing.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_require</p> | int | no | <ul><li>401</li><li>403</li></ul> |  |

### Options for nginx_http_config_files > server > location > gunzip_buffers
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| number | <p>The number of buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_gunzip_module.html#gunzip_buffers</p> | int | yes |  |  |
| size | <p>The size of the buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_gunzip_module.html#gunzip_buffers</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > location > gzip_buffers
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| number | <p>The number of buffers.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_buffers</p> | int | yes |  |  |
| size | <p>The size of the buffer.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_buffers</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > location > gzip_proxied
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable response compression for all proxies.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_proxied</p> | bool | no |  | False |
| any | <p>Whether to enable response compression for any proxy.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_proxied</p> | bool | no |  | False |
| conditions | <p>The list of proxies for which to enable response compression.</p><p>https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_proxied</p> | list of 'str' | no | <ul><li>expired</li><li>no-cache</li><li>no-store</li><li>private</li><li>no_last_modified</li><li>no_etag</li><li>auth</li></ul> |  |

### Options for nginx_http_config_files > server > location > add_header
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| field | <p>The header field to add.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_header</p> | str | yes |  |  |
| value | <p>The header value to add.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_header</p> | str | yes |  |  |
| always | <p>Whether to add the header to all responses.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_header</p> | bool | no |  | False |

### Options for nginx_http_config_files > server > location > add_trailer
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| field | <p>The trailer field to add.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_trailer</p> | str | yes |  |  |
| value | <p>The trailer value to add.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_trailer</p> | str | yes |  |  |
| always | <p>Whether to add the trailer to all responses.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_trailer</p> | bool | no |  | False |

### Options for nginx_http_config_files > server > location > expires
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable expiration.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | bool | no |  | False |
| max | <p>Whether to set the expiration to the maximum value.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | bool | no |  | False |
| epoch | <p>Whether to set the expiration to the minimum value and disable caching that way.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | bool | no |  | False |
| modified | <p>Whether to set the expiration relative to the last modification time.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | bool | no |  |  |
| time | <p>The time to set the expiration.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | str | no |  |  |

### Options for nginx_http_config_files > server > location > limit_req
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| zone | <p>The shared memory zone for rate limiting.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req</p> | str | yes |  |  |
| burst | <p>The maximum burst size.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req</p> | int | no |  |  |
| nodelay | <p>Whether to delay requests exceeding the rate limit.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req</p> | bool | no |  | False |
| delay | <p>The time period to delay requests exceeding the rate limit.</p><p>https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req</p> | str | no |  |  |

### Options for nginx_http_config_files > server > location > access_log
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable access logging.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | bool | no |  | False |
| logs | <p>The list of access log files.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | list of dicts of 'logs' options | no |  |  |

### Options for nginx_http_config_files > server > location > access_log > logs
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| path | <p>The path to the access log file.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | path | yes |  |  |
| format | <p>The log format.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |
| buffer | <p>The buffer size for the access log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |
| gzip | <p>The gzip configuration for the access log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | dict of 'gzip' options | no |  |  |
| flush | <p>The time period to flush the access log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |
| if | <p>The condition under which to log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |

### Options for nginx_http_config_files > server > location > access_log > logs > gzip
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| level | <p>The compression level.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | int | no |  |  |

### Options for nginx_http_config_files > server > location > open_log_file_cache
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable the open log file cache.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#open_log_file_cache</p> | bool | no |  | False |
| max | <p>The maximum number of open log files to cache.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#open_log_file_cache</p> | int | no |  |  |
| inactive | <p>The time period to keep an open log file in the cache.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#open_log_file_cache</p> | str | no |  |  |
| min_uses | <p>The minimum number of uses for an open log file to keep it in the cache.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#open_log_file_cache</p> | int | no |  |  |
| valid | <p>The time period to consider a cached open log file valid.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#open_log_file_cache</p> | str | no |  |  |

### Options for nginx_http_config_files > server > location > error_log
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| file | <p>The path to the error log file.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#error_log</p> | path | no |  |  |
| level | <p>The log level for the error log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#error_log</p> | str | no | <ul><li>debug</li><li>info</li><li>notice</li><li>warn</li><li>error</li><li>crit</li><li>alert</li><li>emerg</li></ul> |  |

### Options for nginx_http_config_files > server > location > mirror
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable mirroring.</p><p>https://nginx.org/en/docs/http/ngx_http_mirror_module.html#mirror</p> | bool | no |  | False |
| uris | <p>The list of URIs to mirror.</p><p>https://nginx.org/en/docs/http/ngx_http_mirror_module.html#mirror</p> | list of 'str' | no |  |  |

### Options for nginx_http_config_files > server > location > return
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| code | <p>The status code to return.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#return</p> | int | no |  |  |
| url | <p>The URI to return.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#return</p> | str | no |  |  |
| text | <p>The text to return.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#return</p> | str | no |  |  |

### Options for nginx_http_config_files > server > location > rewrite
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| regex | <p>The regular expression to match.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite</p> | str | yes |  |  |
| replacement | <p>The replacement value.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite</p> | str | yes |  |  |
| flags | <p>The flags for the rewrite rule.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite</p> | str | no |  |  |

### Options for nginx_http_config_files > server > location > set
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| variable | <p>The variable to set.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#set</p> | str | yes |  |  |
| value | <p>The value to set.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#set</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > location > sub_filter
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| pattern | <p>The pattern to match.</p><p>https://nginx.org/en/docs/http/ngx_http_sub_module.html#sub_filter</p> | str | yes |  |  |
| replacement | <p>The replacement value.</p><p>https://nginx.org/en/docs/http/ngx_http_sub_module.html#sub_filter</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > location > if
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| condition | <p>The condition to evaluate.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#if</p> | str | yes |  |  |
| error_page | <p>The error page configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#error_page</p> | list of dicts of 'error_page' options | no |  |  |
| limit_rate | <p>The rate limit for the response.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#limit_rate</p> | str | no |  |  |
| limit_rate_after | <p>The number of bytes after which to apply the rate limit.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#limit_rate_after</p> | str | no |  |  |
| root | <p>The root directory for the server.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#root</p> | path | no |  |  |
| sendfile | <p>Whether to use the sendfile system call.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#sendfile</p> | bool | no |  |  |
| proxy_pass | <p>The URI to which to pass the request.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass</p> | str | no |  |  |
| grpc_pass | <p>The URI to which to pass the request.</p><p>https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_pass</p> | str | no |  |  |
| add_header | <p>The list of headers to add to the response.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_header</p> | list of dicts of 'add_header' options | no |  | [] |
| add_trailer | <p>The list of trailers to add to the response.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_trailer</p> | list of dicts of 'add_trailer' options | no |  | [] |
| expires | <p>The expiration configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | dict of 'expires' options | no |  |  |
| access_log | <p>The access log configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | dict of 'access_log' options | no |  |  |
| rewrite_log | <p>Whether to enable rewrite logging.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite_log</p> | bool | no |  |  |
| uninitialized_variable_warn | <p>Whether to warn about uninitialized variables.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#uninitialized_variable_warn</p> | bool | no |  |  |
| custom_directives | <p>The list of custom directives to include in the configuration.</p> | list of 'str' | no |  | [] |

### Options for nginx_http_config_files > server > location > if > error_page
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| codes | <p>The list of error codes for which this directive applies.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#error_page</p> | list of 'int' | yes |  |  |
| response | <p>The response code configuration when the specified error codes would normally occur.</p><p>If not defined, the response code from the redirected URI is used.</p><p>If defined, this must be either `=` or `=XXX` with XXX being a response code.</p><p>If set to `=`, the original response code is retained.</p><p>If set to `=XXX`, the response code is replaced with `XXX`.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#error_page</p> | str | no |  |  |
| uri | <p>The URI of the error page.</p><p>Alternatively, this can be set to `@` followed by the name of a location block to reference it.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#error_page</p> | str | yes |  |  |

### Options for nginx_http_config_files > server > location > if > add_header
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| field | <p>The header field to add.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_header</p> | str | yes |  |  |
| value | <p>The header value to add.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_header</p> | str | yes |  |  |
| always | <p>Whether to add the header to all responses.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_header</p> | bool | no |  | False |

### Options for nginx_http_config_files > server > location > if > add_trailer
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| field | <p>The trailer field to add.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_trailer</p> | str | yes |  |  |
| value | <p>The trailer value to add.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_trailer</p> | str | yes |  |  |
| always | <p>Whether to add the trailer to all responses.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#add_trailer</p> | bool | no |  | False |

### Options for nginx_http_config_files > server > location > if > expires
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable expiration.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | bool | no |  | False |
| max | <p>Whether to set the expiration to the maximum value.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | bool | no |  | False |
| epoch | <p>Whether to set the expiration to the minimum value and disable caching that way.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | bool | no |  | False |
| modified | <p>Whether to set the expiration relative to the last modification time.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | bool | no |  |  |
| time | <p>The time to set the expiration.</p><p>https://nginx.org/en/docs/http/ngx_http_headers_module.html#expires</p> | str | no |  |  |

### Options for nginx_http_config_files > server > location > if > access_log
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable access logging.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | bool | no |  | False |
| logs | <p>The list of access log files.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | list of dicts of 'logs' options | no |  |  |

### Options for nginx_http_config_files > server > location > if > access_log > logs
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| path | <p>The path to the access log file.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | path | yes |  |  |
| format | <p>The log format.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |
| buffer | <p>The buffer size for the access log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |
| gzip | <p>The gzip configuration for the access log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | dict of 'gzip' options | no |  |  |
| flush | <p>The time period to flush the access log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |
| if | <p>The condition under which to log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |

### Options for nginx_http_config_files > server > location > if > access_log > logs > gzip
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| level | <p>The compression level.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | int | no |  |  |

### Options for nginx_http_config_files > server > location > limit_except
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| methods | <p>The list of HTTP methods to limit.</p><p>https://nginx.org/en/docs/http/ngx_http_core_module.html#limit_except</p> | list of 'str' | yes |  |  |
| proxy_pass | <p>The URI to which to pass the request.</p><p>https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass</p> | str | no |  |  |
| allow | <p>The list of IP addresses or CIDR blocks to allow.</p><p>https://nginx.org/en/docs/http/ngx_http_access_module.html#allow</p> | list of 'str' | no |  | [] |
| deny | <p>The list of IP addresses or CIDR blocks to deny.</p><p>https://nginx.org/en/docs/http/ngx_http_access_module.html#deny</p> | list of 'str' | no |  | [] |
| auth_basic | <p>The basic authentication realm.</p><p>If set to `off`, disables basic authentication.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_basic_module.html#auth_basic</p> | str | no |  |  |
| auth_basic_user_file | <p>The path to the basic authentication user file.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_basic_module.html#auth_basic_user_file</p> | path | no |  |  |
| auth_jwt | <p>The JWT authentication configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt</p> | dict of 'auth_jwt' options | no |  |  |
| auth_jwt_key_file | <p>The path to the JWT key file.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_key_file</p> | path | no |  |  |
| auth_jwt_key_request | <p>The URI to request the JWT key.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_key_request</p> | str | no |  |  |
| auth_jwt_type | <p>The JWT type.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_type</p> | str | no | <ul><li>signed</li><li>encrypted</li><li>nested</li></ul> |  |
| auth_jwt_require | <p>The list of claims required in the JWT.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_require</p> | list of dicts of 'auth_jwt_require' options | no |  | [] |
| access_log | <p>The access log configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | dict of 'access_log' options | no |  |  |
| custom_directives | <p>The list of custom directives to include in the configuration.</p> | list of 'str' | no |  | [] |

### Options for nginx_http_config_files > server > location > limit_except > auth_jwt
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable JWT authentication.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt</p> | bool | no |  | False |
| realm | <p>The JWT authentication realm.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt</p> | str | no |  |  |
| token | <p>The JWT token.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt</p> | str | no |  |  |

### Options for nginx_http_config_files > server > location > limit_except > auth_jwt_require
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| values | <p>The list of values for the claim.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_require</p> | list of 'str' | yes |  |  |
| error | <p>The error code to return if the claim is missing.</p><p>https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html#auth_jwt_require</p> | int | no | <ul><li>401</li><li>403</li></ul> |  |

### Options for nginx_http_config_files > server > location > limit_except > access_log
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| off | <p>Whether to disable access logging.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | bool | no |  | False |
| logs | <p>The list of access log files.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | list of dicts of 'logs' options | no |  |  |

### Options for nginx_http_config_files > server > location > limit_except > access_log > logs
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| path | <p>The path to the access log file.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | path | yes |  |  |
| format | <p>The log format.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |
| buffer | <p>The buffer size for the access log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |
| gzip | <p>The gzip configuration for the access log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | dict of 'gzip' options | no |  |  |
| flush | <p>The time period to flush the access log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |
| if | <p>The condition under which to log.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | str | no |  |  |

### Options for nginx_http_config_files > server > location > limit_except > access_log > logs > gzip
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| level | <p>The compression level.</p><p>https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log</p> | int | no |  |  |

### Options for nginx_http_config_files > server > if
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| condition | <p>The condition to evaluate.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#if</p> | str | yes |  |  |
| rewrite_log | <p>Whether to enable rewrite logging.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite_log</p> | bool | no |  |  |
| uninitialized_variable_warn | <p>Whether to warn about uninitialized variables.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#uninitialized_variable_warn</p> | bool | no |  |  |
| break | <p>Whether to break rewrite processing.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#break</p> | bool | no |  |  |
| return | <p>The return configuration.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#return</p> | dict of 'return' options | no |  |  |
| rewrite | <p>The list of rewrite rules.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite</p> | list of dicts of 'rewrite' options | no |  | [] |
| set | <p>The list of variables to set.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#set</p> | list of dicts of 'set' options | no |  | [] |
| custom_directives | <p>The list of custom directives to include in the configuration.</p> | list of 'str' | no |  | [] |

### Options for nginx_http_config_files > server > if > return
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| code | <p>The status code to return.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#return</p> | int | no |  |  |
| url | <p>The URI to return.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#return</p> | str | no |  |  |
| text | <p>The text to return.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#return</p> | str | no |  |  |

### Options for nginx_http_config_files > server > if > rewrite
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| regex | <p>The regular expression to match.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite</p> | str | yes |  |  |
| replacement | <p>The replacement value.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite</p> | str | yes |  |  |
| flags | <p>The flags for the rewrite rule.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite</p> | str | no |  |  |

### Options for nginx_http_config_files > server > if > set
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| variable | <p>The variable to set.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#set</p> | str | yes |  |  |
| value | <p>The value to set.</p><p>https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#set</p> | str | yes |  |  |

### Options for nginx_http_config_files > split_clients
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| string | <p>The string to split.</p><p>https://nginx.org/en/docs/http/ngx_http_split_clients_module.html#split_clients</p> | str | yes |  |  |
| variable | <p>The variable to use for splitting.</p><p>https://nginx.org/en/docs/http/ngx_http_split_clients_module.html#split_clients</p> | str | yes |  |  |
| contents | <p>The list of split clients content.</p><p>https://nginx.org/en/docs/http/ngx_http_split_clients_module.html#split_clients</p> | list of dicts of 'contents' options | yes |  |  |
| custom_directives | <p>The list of custom directives to include in the configuration.</p> | list of 'str' | no |  | [] |

### Options for nginx_http_config_files > split_clients > contents
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| percentage | <p>The percentage of clients to which the value is assigned.</p><p>The last percentage can be set to `*` to assign the value to all remaining clients.</p><p>https://nginx.org/en/docs/http/ngx_http_split_clients_module.html#split_clients</p> | str | yes |  |  |
| value | <p>The value to be assigned.</p><p>https://nginx.org/en/docs/http/ngx_http_split_clients_module.html#split_clients</p> | str | yes |  |  |

### Options for nginx_http_config_files > upstream
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| name | <p>The name of the upstream block.</p> | str | yes |  |  |
| server | <p>The list of server declarations.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#server</p> | list of dicts of 'server' options | no |  | [] |
| zone | <p>The shared memory zone for the upstream block.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#zone</p> | dict of 'zone' options | no |  |  |
| load_balancing_method | <p>The load balancing method for the upstream block.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#hash</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#ip_hash</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#least_conn</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#random</p> | str | no | <ul><li>hash</li><li>ip_hash</li><li>least_conn</li><li>random</li></ul> |  |
| hash | <p>The hash key for the upstream block.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#hash</p> | dict of 'hash' options | no |  |  |
| random | <p>The random choice method for the upstream block.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#random</p> | dict of 'random' options | no |  |  |
| keepalive | <p>The maximum number of idle keepalive connections to keep open.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#keepalive</p> | int | no |  |  |
| keepalive_requests | <p>The maximum number of requests to allow on a keepalive connection.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#keepalive_requests</p> | int | no |  |  |
| keepalive_time | <p>The time period over which to keep a keepalive connection open.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#keepalive_time</p> | str | no |  |  |
| keepalive_timeout | <p>The timeout for keepalive connections.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#keepalive_timeout</p> | str | no |  |  |

### Options for nginx_http_config_files > upstream > server
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| address | <p>The address of the server.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#server</p> | str | yes |  |  |
| weight | <p>The weight of the server.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#server</p> | int | no |  |  |
| max_conns | <p>The maximum number of connections to the server.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#server</p> | int | no |  |  |
| max_fails | <p>The number of failed attempts within the fail_timeout period before the server is considered failed.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#server</p> | int | no |  |  |
| fail_timeout | <p>The time period over which to evaluation the server for failure.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#server</p> | str | no |  |  |
| backup | <p>Whether the server is a backup server.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#server</p> | bool | no |  | False |
| down | <p>Whether the server is initially considered down.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#server</p> | bool | no |  | False |

### Options for nginx_http_config_files > upstream > zone
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| name | <p>The name of the shared memory zone.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#zone</p> | str | yes |  |  |
| size | <p>The size of the shared memory zone.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#zone</p> | str | no |  |  |

### Options for nginx_http_config_files > upstream > hash
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| key | <p>The key for the hash.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#hash</p> | str | yes |  |  |
| consistent | <p>Whether the hash is consistent.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#hash</p> | bool | no |  | False |

### Options for nginx_http_config_files > upstream > random
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| two | <p>If defined, two random choices are made.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#random</p> | dict of 'two' options | no |  |  |

### Options for nginx_http_config_files > upstream > random > two
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| method | <p>The method for deciding between the two random choices.</p><p>https://nginx.org/en/docs/http/ngx_http_upstream_module.html#random</p> | str | no |  |  |


## License
MIT

## Author and Project Information
Jim Tarpley (@trippsc2)
<!-- END_ANSIBLE_DOCS -->
