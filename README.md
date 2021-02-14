Based on blog https://medium.com/learning-cloud-native-go/lets-get-it-started-dc4634ef03b


# build the compose

```
sqr@DESKTOP-QVAN5NN:meApp$ docker-compose build
Building app
Step 1/12 : FROM golang:1.13-alpine as build-env
 ---> 5863598a981a
Step 2/12 : WORKDIR /myapp
 ---> Using cache
 ---> f2d3316877b8
Step 3/12 : RUN apk update && apk add --no-cache gcc musl-dev git
 ---> Using cache
 ---> b903dfd1e604
Step 4/12 : COPY go.mod go.sum ./
 ---> Using cache
 ---> cce1a040ddc8
Step 5/12 : RUN go mod download
 ---> Using cache
 ---> f21ffaec6454
Step 6/12 : COPY . .
 ---> 616596d5ad51
Step 7/12 : RUN go build -ldflags '-w -s' -a -o ./bin/app ./cmd/app
 ---> Running in 669a3abd341a
Removing intermediate container 669a3abd341a
 ---> f692040b9ec9

Step 8/12 : FROM alpine
 ---> e50c909a8df2
Step 9/12 : RUN apk update && apk add --no-cache bash
 ---> Using cache
 ---> ed0f8a9fae17
Step 10/12 : COPY --from=build-env /myapp/bin/app /myapp/
 ---> 4e2c1c89578f
Step 11/12 : EXPOSE 8080
 ---> Running in cf9a48f39511
Removing intermediate container cf9a48f39511
 ---> 02186103da19
Step 12/12 : CMD ["/myapp/app"]FROM golang:1.13-alpine as build-env
 ---> Running in 297f11fb8b96
Removing intermediate container 297f11fb8b96
 ---> a449ebb6bead

Successfully built a449ebb6bead
Successfully tagged meapp_app:latest
```

# run it
```
sqr@DESKTOP-QVAN5NN:meApp$ docker-compose up
WARNING: The Docker Engine you're using is running in swarm mode.

Compose does not use swarm mode to deploy services to multiple nodes in a swarm. All containers will be scheduled on the current node.

To deploy your application across the swarm, use `docker stack deploy`.

Recreating meapp_app_1 ... done
Attaching to meapp_app_1
app_1  | 2021/02/14 03:45:05 Starting server :8080
```

# test it
```
sqr@DESKTOP-QVAN5NN:meApp$ curl localhost:8080
Hello World!
```
