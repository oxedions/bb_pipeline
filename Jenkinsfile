pipeline {
    agent none

    stages {
        stage('build_packages') {
            parallel {
                stage('build_x86_64_packages_RedHat_8') {
                    agent {
                        label "x86_64"
                    }
                    steps {
                        sh "podman pull docker.io/rockylinux/rockylinux:8"
                        sh "podman build --no-cache --tag rockylinux_8_build -f packages_build/dockerfile_rockylinux"
                        sh "podman run -it --rm -v /nfs/el8:/root/rpmbuild rockylinux_8_build 1 RedHat 8"
                        sh "podman run -it --rm -v /nfs/el8:/root/rpmbuild rockylinux_8_build 2 RedHat 8"
                        sh "podman run -it --rm -v /nfs/el8:/root/rpmbuild rockylinux_8_build 3 RedHat 8"
                        sh "podman run -it --rm -v /nfs/el8:/root/rpmbuild rockylinux_8_build 4 RedHat 8"
                        sh "podman run -it --rm -v /nfs/el8:/root/rpmbuild rockylinux_8_build 5 RedHat 8"
                        sh "podman run -it --rm -v /nfs/el8:/root/rpmbuild rockylinux_8_build 8 RedHat 8"
                        sh "podman run -it --rm -v /nfs/el8:/root/rpmbuild rockylinux_8_build 10 RedHat 8 1.4"
                        sh "podman run -it --rm -v /nfs/el8:/root/rpmbuild rockylinux_8_build 14 RedHat 8"
                        sh "podman run -it --rm -v /nfs/el8:/root/rpmbuild rockylinux_8_build 15 RedHat 8"
                        sh "podman run -it --rm -v /nfs/el8:/root/rpmbuild rockylinux_8_build 16 RedHat 8"
                    }
                    post {
                        always {
                            sh "echo podman rmi rockylinux_8_build"
                        }
                    }
                }
            }
        }
    }
}
