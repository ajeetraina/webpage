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
