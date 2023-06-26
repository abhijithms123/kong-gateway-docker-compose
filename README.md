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
 
          $docker compose up

to set up konga

1. add the konga image to the docker compose file.
   
            konga:
              image: pantsel/konga
              environment:
                # TOKEN_SECRET: ${KONGA_TOKEN_SECRET}
                # DB_ADAPTER: ${KONG_DATABASE}
                # DB_HOST: ${KONGA_DB_HOST}
                # DB_PORT: ${KONGA_DB_PORT}
                # DB_DATABASE: ${KONGA_DB_NAME}
                # DB_USER: ${KONGA_DB_USERNAME}
                # DB_PASSWORD: ${KONGA_DB_PASSWORD}
                # NODE_ENV: ${KONGA_ENV}
                KONGA_HOOK_TIMEOUT: 10000
                restart: on-failure
                ports:
                - 9000:1337
                # depends_on:
                # - db
                networks:
                - kong-net
   
3. build the container
   
        $docker compose up -d

#setting up KONGA.

   1.create an admin
   
  2.sign in with the admin credentials
  
  3.add a connection to the admin api.
  
    .set the url of the connection as: http://kong:8001
    .since both the containers are in the same network("kong-net") the container name is given as the host.




     
     
   
   
