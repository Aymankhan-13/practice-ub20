pipeline {
  agent none
  stages {
    stage('BuildAndTest') {
      matrix {
        agent {
            docker { image "${DOCKER_IMAGE}" }
        }
        axes {
          axis {
            name 'DOCKER_IMAGE'
            values 'vtpl/ubuntu_build:20.04'
          }
        }
        stages {
          stage('Build') {
            environment {
                          VCPKG_DEFAULT_BINARY_CACHE = '${WORKSPACE}/vcpkg_cache'
                        }
            steps {
                  sh "cat /etc/os-release"
                  dir('vcpkg_cache') {
                      writeFile file:'dummy', text:''
                  }
                  dir('thirdparty/vcpkg') {
                      checkout([$class: 'GitSCM', branches: [[name: 'fb02a785618a1fbaced32b206171e09fd57d4622']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Microsoft/vcpkg.git']]])
                  }
                  cmakeBuild buildDir: 'build', buildType: 'Release', cleanBuild: true, cmakeArgs: '-DVCPKG_OVERLAY_TRIPLETS:STRING=${WORKSPACE} -DVCPKG_TARGET_TRIPLET:STRING=x64-mixed -DCMAKE_TOOLCHAIN_FILE:STRING=${WORKSPACE}/thirdparty/vcpkg/scripts/buildsystems/vcpkg.cmake -DCMAKE_C_COMPILER_LAUNCHER:STRING=ccache -DCMAKE_CXX_COMPILER_LAUNCHER:STRING=ccache -DCMAKE_EXPORT_COMPILE_COMMANDS:BOOL=TRUE -DCMAKE_BUILD_TYPE:STRING=Release -DCMAKE_C_COMPILER:FILEPATH=/usr/bin/clang -DCMAKE_CXX_COMPILER:FILEPATH=/usr/bin/clang++', generator: 'Ninja', installation: 'InSearchPath', steps: [[withCmake: true]]
                  archiveArtifacts artifacts: 'build/vtpl_agent/vtpl_agent', fingerprint: true, followSymlinks: false, onlyIfSuccessful: true
              }
            }
          }
        }
      }
    }
  }

