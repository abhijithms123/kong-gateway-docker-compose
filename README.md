# kong-gateway-docker-compose

1.Build the custom image from the Dockerfile in the directory

    $ docker build -t my-custom-kong-image .
    
2.use that image in the kong service in the docker-compose.yml file

        kong:
          image: my-custom-kong-image
          user: "${KONG_USER:-kong}"
          
3.Set the environment variable KONG_PLUGINS in the kong container.


        kong:
           image: my-custom-kong-image
           user: "${KONG_USER:-kong}"
           environment:
           <<: *kong-env
           KONG_ADMIN_ACCESS_LOG: /dev/stdout
           KONG_ADMIN_ERROR_LOG: /dev/stderr
           KONG_PROXY_LISTEN: "${KONG_PROXY_LISTEN:-0.0.0.0:8000}"
           KONG_ADMIN_LISTEN: "${KONG_ADMIN_LISTEN:-0.0.0.0:8001}"
           KONG_PROXY_ACCESS_LOG: /dev/stdout
           KONG_PROXY_ERROR_LOG: /dev/stderr
           KONG_PREFIX: ${KONG_PREFIX:-/var/run/kong}
           KONG_DECLARATIVE_CONFIG: "/opt/kong/kong.yaml"
           KONG_PLUGINS: bundled,kong-jwt2header
 4.Run the docker container.
    docker compose up
     
     
   
   
