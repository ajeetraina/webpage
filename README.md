# A Simple HTML Webpage for Demonstrating CI-CD with Docker, Jenkins & GitHub 

## Cloning the Repository

```
$git clone https://github.com/ajeetraina/webpage
```

## Building Docker Image

```
$cd webpage
$docker build -t ajeetraina/webpage .
```

## Running the Container

```
$docker run -d -p 80:80 ajeetraina/webpage
```

## Jenkinsfile

```
node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("ajeetraina/webpage")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * Just an example */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Run image') {
        /* Run an image to start services */
        docker.image("ajeetraina/webpage").run('-p 80:80')
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
```
