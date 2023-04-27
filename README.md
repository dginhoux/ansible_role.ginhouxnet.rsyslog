# ROLE dginhoux.rsyslog



## DESCRIPTION

This ansible role configure rsyslog and only use new syntax !<br />
<br />
WARNING : It doesn't check rsyslog version, so if you're using and older version of rsyslog who don't support the new syntax, you'll get fews errors. But, this role is "locked" to be used on recent linux version, with recent rsyslog version



## REQUIREMENTS

#### SUPPORTED PLATFORMS

This role require a supported platform.<br />
It will skip node with unsupported platform to avoid any compatibility problem.<br />
This behaviour can be bypassed by settings the following variable `asserts_bypass=True`.

| Platform | Versions |
|----------|----------|
| Debian | buster, bullseye |
| Fedora | 33, 34, 35, 36, 37, 38 |
| EL | 7, 8 |

#### ANSIBLE VERSION

Ansible >= 2.13

#### DEPENDENCIES

None.



## INSTALLATION

#### ANSIBLE GALAXY

```shell
ansible-galaxy install dginhoux.rsyslog
```
#### GIT

```shell
git clone https://github.com/dginhoux/ansible_role.rsyslog dginhoux.rsyslog
```


## USAGE

#### EXAMPLE PLAYBOOK

```yaml
- hosts: all
  roles:
    - name: start role dginhoux.rsyslog
      ansible.builtin.include_role:
        name: dginhoux.rsyslog
```


## VARIABLES

#### DEFAULT VARIABLES

Defaults variables defined in `defaults/main.yml` : 

```yaml
rsyslog_configure: install
rsyslog_packages:
  - rsyslog
rsyslog_services:
  - rsyslog
rsyslog_enable: true

rsyslog_work_dir: /var/spool/rsyslog
rsyslog_main_conf_file: /etc/rsyslog.conf
rsyslog_custom_conf_dir: /etc/rsyslog.d
rsyslog_include_conf_files_dir: /etc/rsyslog.d/*.conf

rsyslog_global_conf:
  umask: "0022"
  workDirectory: "{{ rsyslog_work_dir }}"
  net.enableDNS: "on"
  preserveFQDN: "on"
  processInternalMessages: "off"

rsyslog_modules_list:
  - imuxsock
  - imjournal
  - imklog
  - immark
  - imfile
  # - imudp
  # - imtcp
  - omfile
  - omfwd
  - omusrmsg

rsyslog_module_imuxsock_conf:
  SysSock.Use: "on"
rsyslog_module_imuxsock_input: []

rsyslog_module_imjournal_conf:
  StateFile: imjournal.state
  IgnoreNonValidStatefile: "on"

rsyslog_module_imklog_conf: []

rsyslog_module_immark_conf: []

rsyslog_module_imudp_conf: []
rsyslog_module_imudp_input:
  - port: 514

rsyslog_module_imtcp_conf: []
rsyslog_module_imtcp_input:
  - port: 514

rsyslog_module_imfile_conf: []
rsyslog_module_imfile_input: []

rsyslog_module_omfile_conf:
  ansible.builtin.template: RSYSLOG_TraditionalFileFormat
  fileOwner: root
  fileGroup: adm
  FileCreateMode: "0640"
  dirOwner: root
  dirGroup: adm
  DirCreateMode: "0755"
rsyslog_module_omfile_action:
  # - filter: "*.info;mail.none;authpriv.none;cron.none"
  - filter: "*.=info;*.=notice;*.=warn;auth,authpriv,cron,daemon,mail,uucp,news,lpr.none"
    options:
      File: /var/log/messages
    pre_line: ""
    post_line: ""
  - filter: "auth,authpriv.*"
    options:
      File: /var/log/auth.log
  - filter: "mail.*"
    options:
      File: /var/log/maillog
  - filter: "cron.*"
    options:
      File: /var/log/cron.log
  - filter: "daemon.*"
    options:
      File: /var/log/daemon.log
  - filter: "uucp,news.crit"
    options:
      File: /var/log/spooler
  - filter: "lpr.*"
    options:
      File: /var/log/lpr.log
  # - filter: "*.=debug;auth,authpriv.none;mail.none"
  #   options:
  #     File: /var/log/debug
  - filter: "local7.*"
    options:
      File: /var/log/boot.log
  - filter: "kern.*"
    options:
      File: /var/log/kern.log
    pre_line: "if not ($msg contains 'iptables') then {"
    post_line: "}"
  - filter: "kern.info"
    options:
      File: /var/log/iptables.log
    pre_line: "if ($msg contains 'iptables') then {"
    post_line: "}"

rsyslog_module_omusrmsg_conf: []
rsyslog_module_omusrmsg_action:
  - filter: "*.emerg"
    options:
      users: "*"

rsyslog_module_omfwd_conf:
  ansible.builtin.template: RSYSLOG_TraditionalFileFormat
rsyslog_module_omfwd_action:
  []
  # - filter: kern.warning
  #   options:
  #     protocol: udp
  #     target: rsyslog.infra.ginhoux.net
  #     port: 514
  # - filter: daemon.warning
  #   options:
  #     protocol: udp
  #     target: rsyslog.infra.ginhoux.net
  #     port: 514

rsyslog_custom_configure: true
rsyslog_custom_list:
  - name: iptables
  # definition: >
  #             if ($msg contains 'iptables') then {
  #               action(
  #                      type="omfile"
  #                      File="/var/log/iptables"
  #                      )
  #             }
  - name: forward_rsyslog_infra_ginhoux_net
    rule:
      - filter: kern.warning
        mode: action
        type: omfwd
        options:
          protocol: udp
          target: rsyslog.infra.ginhoux.net
          port: 51
        pre_line: ""
        post_line: ""
      - filter: daemon.warning
        mode: action
        type: omfwd
        options:
          protocol: udp
          target: rsyslog.infra.ginhoux.net
          port: 514
        pre_line: ""
        post_line: ""
  # - name: toto
  #   module:
  #     - name: gggg
  #       # conf:
  #       #   load: free
  #       #   header: oui
  #       #   footer: non
  #     - name: hhhhh
  #       conf:
  #         load: rrr
  #         free: oui
  #         sfr: non
  #   rule:
  #     - filter: "*.debug"
  #       mode: action
  #       type: gggg
  #       # options:
  #       #   File: /var/log/messages
  #     - filter: "*.warn"
  #       mode: input
  #       type: rrr
  #       options:
  #         File: /var/log/warn
  #         FileCreate: "yes"
```

#### DEFAULT OS SPECIFIC VARIABLES

Those variables files are located in `vars/*.yml` are used to handle OS differences.<br />
One of theses is loaded dynamically during role runtime using the `include_vars` module and set OS specifics variable's.

* RedHat Family 

```yaml
rsyslog_default_rules_list:
  # log all kernel messages to the console.
  # - rule: "kern.*"
  #   logpath: "/dev/console"
  # log anything (except mail) of level info or higher ; Don't log private authentication messages!
  - rule: "*.info;mail.none;authpriv.none;cron.none"
    logpath: "/var/log/messages"
  # The authpriv file has restricted access.
  - rule: "authpriv.*"
    logpath: "/var/log/secure"
  # Log all the mail messages in one place.
  - rule: "mail.*"
    logpath: "-/var/log/maillog"
  # Log cron stuff
  - rule: "cron.*"
    logpath: "/var/log/cron"
  # Everybody gets emergency messages
  - rule: "*.emerg"
    logpath: ":omusrmsg:*"
  # Save news errors of level crit and higher in a special file.
  - rule: "uucp,news.crit"
    logpath: "/var/log/spooler"
  # Save boot messages also to boot.log
  - rule: "local7.*"
    logpath: "/var/log/boot.log"
```

* Debian Family 

```yaml
rsyslog_default_rules_list:
  # standard logs
  - rule: "auth,authpriv.*"
    logpath: "/var/log/auth.log"
  - rule: "*.*;auth,authpriv.none"
    logpath: "-/var/log/syslog"
  - rule: "#cron.*"
    logpath: "/var/log/cron.log"
  - rule: "daemon.*"
    logpath: "-/var/log/daemon.log"
  - rule: "kern.*"
    logpath: "-/var/log/kern.log"
  - rule: "lpr.*"
    logpath: "-/var/log/lpr.log"
  - rule: "mail.*"
    logpath: "-/var/log/mail.log"
  - rule: "user.*"
    logpath: "-/var/log/user.log"
  # mail logs
  - rule: "mail.info"
    logpath: "-/var/log/mail.info"
  - rule: "mail.warn"
    logpath: "-/var/log/mail.warn"
  - rule: "mail.err"
    logpath: "/var/log/mail.err"
  # "catch-all" log files
  - rule: "*.=debug;auth,authpriv.none;mail.none"
    logpath: "-/var/log/debug"
  - rule: "*.=info;*.=notice;*.=warn;auth,authpriv.none;cron,daemon.none;mail.none"
    logpath: "-/var/log/messages"
  # emergency logs
  - rule: "*.emerg"
    logpath: ":omusrmsg:*"
```


## AUTHOR

Dany GINHOUX - https://github.com/dginhoux



## LICENSE

MIT
