/* Copyright (C) 2021  Alessandro Santoni

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>. */

pipeline {
    agent any
    
    stages {
        stage('Preparation') {
            steps {
                git(
                    url: 'git@github.com:AlessandroSantoni/docker-micropython-jenkins.git',
                    branch: "main"
                )}
        }
        stage('Build') {
            steps {
                sh './make_n_extract_image.sh ${BUILD_NUMBER}'
            }
        }
    }
    
    post {
        success {
            archiveArtifacts artifacts: 'esp8266_micropython_build*.bin', fingerprint: true
        }
        cleanup {
            deleteDir()
        }
    }
}