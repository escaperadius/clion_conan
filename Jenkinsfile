 node {
    def conanClient = Artifactory.newConanClient()
    def server = Artifactory.server SERVER_ID
    def buildInfo = Artifactory.newBuildInfo()
    stage('init') {
        git url :'https://github.com/'+ REPO + '/clion_conan.git'
        buildInfo.env.collect()
    }
    stage ('resolve') {
	      dir('boost_poco_md5') {
          // Add new remote repository and new user to conan configuration.
          // The server URL and the user details are taken from the server parameters.
          // The add new server returns the server unique identifier as a return value.
          conanClient.run(command: "remote remove conan-center")
          conanClient.run(command: "remote remove conan-transit")
          String serverName = conanClient.remote.add server: server, repo: "conan-local"
          String serverExtName = conanClient.remote.add server: server, repo: "conan-ext-local"
          conanClient.run(command: "install --build missing", buildInfo: buildInfo)
          String command = "upload * --all -r ${serverExtName} --confirm"
        }
    }
    stage ('build') {
        dir('boost_poco_md5') {
          sh 'rm -rf bin'
          sh 'ls'
          sh 'cmake . -DCMAKE_BUILD_TYPE=Release'
          sh 'ls'
          sh 'cmake --build .'
          sh 'ls bin'
          sh 'mv bin/demo bin/app-'+env.BUILD_NUMBER+'.exe'
          def uploadSpec = """{
            "files": [
              {
                "pattern": "bin/*",
                "target": "generic-local/conan/demo/"
              }
            ]
          }"""
          server.upload(uploadSpec, buildInfo)
          server.publishBuildInfo buildInfo
        }
	  }
}
