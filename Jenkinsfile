pipeline {
    agent none

    stages {
        stage('build_repositories') {
            parallel {
                stage('build_x86_64_repositories_RedHat_8') {
                    agent {
                        label "x86_64"
                    }
                    steps {
                        sh '''
                            podman run -it --rm -v /nfs/:/nfs/ rockylinux/rockylinux:8 /bin/bash -c ' \
                            dnf install -y wget yum-utils createrepo; \
                            mkdir -p /nfs/repositories/el8/x86_64/; \
                            wget http://bluebanquise.com/repository/releases/1.5-dev/el8/x86_64/bluebanquise/bluebanquise.repo -P /etc/yum.repo.d/ ;\
                            reposync --repoid=bluebanquise -p /nfs/repositories/el8/x86_64/
                            rsync -a -v --ignore-existing /nfs/build/x86_64/el8/* /nfs/repositories/el8/x86_64/packages/
                            createrepo /nfs/repositories/el8/x86_64/
                            '
                        '''
                        sh '/bin/bash exit 1'
                    }
                }
            }
        }
        stage('build_packages') {
            parallel {
                stage('build_x86_64_packages_Ubuntu_20.04') {
                    agent {
                        label "x86_64"
                    }
                    steps {
                        sh "podman pull docker.io/ubuntu:20.04"
                        sh "podman build --no-cache --tag ubuntu_20.04_build -f packages_build/dockerfile_ubuntu_20.04"
                        sh "podman run -it --rm -v /nfs/x86_64/ubuntu2004:/root/debbuild ubuntu_20.04_build 1 Ubuntu 20.04"
                        sh "podman run -it --rm -v /nfs/x86_64/ubuntu2004:/root/debbuild ubuntu_20.04_build 2 Ubuntu 20.04"
                        sh "podman run -it --rm -v /nfs/x86_64/ubuntu2004:/root/debbuild ubuntu_20.04_build 3 Ubuntu 20.04"
                        sh "podman run -it --rm -v /nfs/x86_64/ubuntu2004:/root/debbuild ubuntu_20.04_build 4 Ubuntu 20.04"
                        sh "podman run -it --rm -v /nfs/x86_64/ubuntu2004:/root/debbuild ubuntu_20.04_build 5 Ubuntu 20.04"
                        sh "podman run -it --rm -v /nfs/x86_64/ubuntu2004:/root/debbuild ubuntu_20.04_build 8 Ubuntu 20.04"
                        //sh "podman run -it --rm -v /nfs/x86_64/ubuntu2004:/root/debbuild ubuntu_20.04_build 10 Ubuntu 20.04 1.4"
                        sh "podman run -it --rm -v /nfs/x86_64/ubuntu2004:/root/debbuild ubuntu_20.04_build 12 Ubuntu 20.04"
                    }
                    post {
                        always {
                            // Remove image, ensure fresh new runs each time
                            sh "podman rmi ubuntu_20.04_build"
                        }
                    }
                }
                stage('build_x86_64_packages_RedHat_8') {
                    agent {
                        label "x86_64"
                    }
                    steps {
                        sh "podman pull docker.io/rockylinux/rockylinux:8"
                        sh "podman build --no-cache --tag rockylinux_8_build -f packages_build/dockerfile_rockylinux"
                        sh "podman run -it --rm -v /nfs/x86_64/el8:/root/rpmbuild/RPMS rockylinux_8_build 1 RedHat 8"
                        sh "podman run -it --rm -v /nfs/x86_64/el8:/root/rpmbuild/RPMS rockylinux_8_build 2 RedHat 8"
                        sh "podman run -it --rm -v /nfs/x86_64/el8:/root/rpmbuild/RPMS rockylinux_8_build 3 RedHat 8"
                        sh "podman run -it --rm -v /nfs/x86_64/el8:/root/rpmbuild/RPMS rockylinux_8_build 4 RedHat 8"
                        sh "podman run -it --rm -v /nfs/x86_64/el8:/root/rpmbuild/RPMS rockylinux_8_build 5 RedHat 8"
                        sh "podman run -it --rm -v /nfs/x86_64/el8:/root/rpmbuild/RPMS rockylinux_8_build 8 RedHat 8"
                        //sh "podman run -it --rm -v /nfs/x86_64/el8:/root/rpmbuild/RPMS rockylinux_8_build 10 RedHat 8 1.4"
                        // Delayed to 1.6 sh "podman run -it --rm -v /nfs/x86_64/el8:/root/rpmbuild/RPMS rockylinux_8_build 14 RedHat 8"
                        sh "podman run -it --rm -v /nfs/x86_64/el8:/root/rpmbuild/RPMS rockylinux_8_build 15 RedHat 8"
                        sh "podman run -it --rm -v /nfs/x86_64/el8:/root/rpmbuild/RPMS rockylinux_8_build 16 RedHat 8"
                        // Check packages can be installed
                        //sh "podman run -it --rm -v /nfs/x86_64/el8:/root/rpmbuild/RPMS rockylinux/rockylinux:8 /bin/bash -c 'dnf install epel-release -y; dnf install dnf-plugins-core -y; dnf config-manager --set-enabled powertools; dnf install -y /root/rpmbuild/RPMS/noarch/*.rpm /root/rpmbuild/RPMS/x86_64/*.rpm'"
                        //sh "podman run -it --rm -v /nfs/x86_64/el8:/root/rpmbuild/RPMS rockylinux/rockylinux:8 /bin/bash -c 'dnf install -y git ; mkdir /infra; git clone https://github.com/oxedions/infrastructure.git /infra ; chmod +x /infra/auto_builder.sh ; auto_builder.sh 0 RedHat 8 ; dnf install -y /root/rpmbuild/RPMS/noarch/*.rpm /root/rpmbuild/RPMS/x86_64/*.rpm'"

                    }
                    post {
                        always {
                            sh "podman rmi rockylinux_8_build"
                        }
                    }
                }
                stage('build_x86_64_packages_RedHat_7') {
                    agent {
                        label "x86_64"
                    }
                    steps {
                        sh "podman pull docker.io/centos:7"
                        sh "podman build --no-cache --tag centos_7_build -f packages_build/dockerfile_centos_7"
                        sh "podman run -it --rm -v /nfs/x86_64/el7:/root/rpmbuild/RPMS centos_7_build 1 RedHat 7"
                        sh "podman run -it --rm -v /nfs/x86_64/el7:/root/rpmbuild/RPMS centos_7_build 2 RedHat 7"
                        sh "podman run -it --rm -v /nfs/x86_64/el7:/root/rpmbuild/RPMS centos_7_build 3 RedHat 7"
                        sh "podman run -it --rm -v /nfs/x86_64/el7:/root/rpmbuild/RPMS centos_7_build 4 RedHat 7"
                        sh "podman run -it --rm -v /nfs/x86_64/el7:/root/rpmbuild/RPMS centos_7_build 5 RedHat 7"
                        sh "podman run -it --rm -v /nfs/x86_64/el7:/root/rpmbuild/RPMS centos_7_build 8 RedHat 7"
                        //sh "podman run -it --rm -v /nfs/x86_64/el7:/root/rpmbuild/RPMS centos_7_build 10 RedHat 7 1.4"
                    }
                    post {
                        always {
                            sh "podman rmi centos_7_build"
                        }
                    }
                }
            }
        }
    }
}
