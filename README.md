# Ansible Role: Java

[![CI](https://github.com/sukhorukovmv/ansible-role-java/workflows/CI/badge.svg)](https://github.com/sukhorukovmv/ansible-role-java/actions?query=workflow%CI)
![Release](https://img.shields.io/github/v/release/sukhorukovmv/ansible-role-java)
![Ansible Role](https://img.shields.io/ansible/role/d/sukhorukovmv/java)
![License](https://img.shields.io/github/license/sukhorukovmv/ansible-role-java)


Installs Java for RedHat/CentOS and Debian/Ubuntu linux servers.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values:

    # The defaults provided by this role are specific to each distribution.
    java_packages:
      - java-1.8.0-openjdk

Set the version/development kit of Java to install, along with any other necessary Java packages. Some other options include are included in the distribution-specific files in this role's 'defaults' folder.

    java_home: ""

If set, the role will set the global environment variable `JAVA_HOME` to this value.

## Dependencies

None.

## Example Playbook (using default package)

    - hosts: servers
      roles:
        - role: sukhorukovmv.java
          become: yes

## Example Playbook (install OpenJDK 8)

For RHEL / CentOS:

    - hosts: server
      roles:
        - role: sukhorukovmv.java
          when: "ansible_os_family == 'RedHat'"
          java_packages:
            - java-1.8.0-openjdk

For Ubuntu < 16.04:

    - hosts: server
      tasks:
        - name: installing repo for Java 8 in Ubuntu
  	      apt_repository: repo='ppa:openjdk-r/ppa'
    
    - hosts: server
      roles:
        - role: sukhorukovmv.java
          when: "ansible_os_family == 'Debian'"
          java_packages:
            - openjdk-8-jdk

## Example Playbook (install OracleJDK 8)

      vars:
        java_oracle_jdk_install: true
        java_version: 8
        java_oracle_jdk_version: "1.{{ java_version }}.0_221"
        java_oracle_jdk_url: "http://nexus/repository/repo/java/jdk/{{ java_oracle_jdk_version }}/jdk-{{ java_oracle_jdk_version }}.tar.gz"

      pre_tasks:
        - name: Update apt cache.
          apt:
            update_cache: true
          when: ansible_os_family == 'Debian'
          changed_when: false

      roles:
        - {role: sukhorukovmv.java}

## License

MIT
