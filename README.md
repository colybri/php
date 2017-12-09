PHP
=====

This role for ansible installs and/or configures php for Nngix or Apache server in Ubuntu system. You can install PHP 5.6 version or higher. Also, you can specific the most important directives to configure your php.ini.

Requirements
------------

This role requires Ansible 2.0 or higher and platform requirements are listed in the metadata file. I will try to add new platforms as soon as posible.

Role Variables
--------------

There is a list of default variables for your php.ini. All the tasks related with these variables have a tag so you can launch the installation or the configuration separately. You can even configure only part of your php file.
```yml
    #tags: [install]
    php_ppa: "ppa:ondrej/php"

    #tags: [configure, timezone]
    php_timezone: Europe/Madrid

    #tags: [configure]
    php_upload_max_filesize: "40M"
    php_post_max_size: "40M"
    php_memory_limit: "1024M"
    php_max_execution_time: 60

    #tags: [configure, secure]
    php_allow_url_fopen: 0
    php_register_globals: Off
    php_magic_quotes_gpc: Off
    php_magic_quotes_sybase: Off

    #tags: [configure, errors]
    php_display_errors: On
    php_display_startup_errors: On
    php_error_reporting: E_ALL

    #tags: [configure, session]
    php_session_cookie_httponly: 0
    php_session_strict_mode: 1

    #tags: [configure, opcache]
    php_opcache_enabled: 1
    php_opcache_revalidate_freq: 2592000
    php_opcache_validate_timestamps: 1
    php_opcache_max_accelerated_files: 20000
    php_opcache_memory_consumption: 192
    php_opcache_interned_strings_buffer: 16
    php_opcache_fast_shutdown: 1
```

Also, you can define the following variables:
```yml
    #tags: [configure]
    php_upload_tmp_dir

    #tags: [configure, errors]
    php_error_log
```

Examples
========

1) Launch only PHP install without any configuration. PHP 7.1 by default will be installed in your hosts.

Type in your terminal

    ansible-playbook - i inventory your-playbook.yml --tags "install"

In your playbook
```yml
    - hosts: all
      roles:
      - role: php
```

The default php packages installed will be:
```yml
    - php7.1-common
    - php7.1-bcmath
    - php7.1-ctype
    - php7.1-curl
    - php7.1-dom
    - php7.1-gd
    - php7.1intl
    - php7.1-mbstring
    - php7.1-mcrypt
    - php7.1-mysql
    - php7.1-xml
    - php7.1-soap
    - php7.1-xsl
    - php7.1-zip
    - php7.1-json
    - php7.1-iconv
    - php7.1-fpm
```
2) But you can customize your own version and packages. Also, you must set php version to install in `php_version` variable whit the format specified below:  
```yml
    - hosts: all
      roles:
      - role: php
        php_version: 7.0
        php_packages:
        - php7.0-curl
        - php7.0-json
        - php7.0-mysql
```
3) Install PHP and configure your php.ini
```yml
    - hosts: all
      roles:
      - role: php
        php_version: 7.0
        php_error_reporting: E_STRICT
        php_upload_tmp_dir: /your/path
        php_error_log: /your/path
        ...
```
4) If you have already installed php, you can just modify your congiguration. Your php version and web server will be automatically detected. Just put the values of your directives in the predefined variables. The rest of the directives that you do not indicate, will adopt the default value. Keep this in mind when you call the role with tags from the command line. If you put a label to modify a directive set, the same thing happens with the variables of the block that you do not indicate.

Type in your terminal

    ansible-playbook - i your-inventory your-playbook.yml --tags "timezone"


In your playbook
```yml
    - hosts: all
        roles:
        - role: php
          php_timezone: 'Europe/Athens'
```



Dependencies
------------

None

License
-------

MIT
