pipeline {

    agent any

    stages {
	
		stage('Clone') {

            steps {

                script {

                    git url: "https://github.com/ramasatrio22/cicddocker.git", branch: "main"
                }

            }

        }

        stage('Pull Nginx Image') {

            steps {

                script {

                    // Pull the Nginx Docker image

                    dockerImage = docker.image('nginx:latest')

                    dockerImage.pull()

                }

            }

        }

        stage('Deploy') {

            steps {

                script {
					sh '''
					set +e
                        SECRET=dckr_pat_5UhfAjy_FWjE5CwFynmHS8v8rOM
	                    echo "$SECRET" | docker login --username praboworama --password-stdin

	                    docker ps --filter "status=running" --format "{{.ID}} {{.Image}}" > test.txt
	                    cat test.txt
	                    grep 'server' test.txt > test1.txt
	                    cat test1.txt
                        containerid=$(cut -d " " -f 1 test1.txt)
                        echo "$containerid"
                        if [ -n "$containerid" ]; then
							docker cp /var/lib/jenkins/workspace/dockerpipeline/. "$containerid":/usr/share/nginx/html
							docker stop $containerid
							docker start server
						else
							docker build -t server /var/lib/jenkins/workspace/dockerpipeline
							docker run --name server -d -p 9090:80 server
                        fi
					'''
					}

				}

			}

		}
	}
