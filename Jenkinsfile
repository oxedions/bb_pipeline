pipeline {
    agent any

    stages {
        stage('build_packages') {
            parallel {
                stage('build_x86_64') {
                    agent {
                        label "gabriel"
                    }
                    steps {
                        sh "podman pull docker.io/rockylinux/rockylinux:8"
                        sh '''
                            cat << EFO > Dockerfile
                            FROM docker.io/rockylinux/rockylinux:8
                            RUN dnf install 'dnf-command(config-manager)'
                            RUN dnf install make rpm-build genisoimage xz xz-devel automake autoconf python36 bzip2-devel openssl-devel zlib-devel readline-devel pam-devel perl-ExtUtils-MakeMaker grub2-tools-extra grub2-efi-x64-modules gcc mariadb mariadb-devel dnf-plugins-core curl-devel net-snmp-devel -y
                            RUN dnf config-manager --set-enabled PowerTools
                            RUN dnf install freeipmi-devel -y
                            RUN dnf groupinstall 'Development Tools' -y
                            RUN 
                            EOF
                        '''
                        sh "podman images"
                        sh "podman run -it centos:latest cat /etc/os-release"
                    }
                }
            }
        }
    }
}
