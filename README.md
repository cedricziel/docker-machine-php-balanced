# Docker runnable PHP Application

Usable with docker-machine. Based on the Heroku PHP Docker image. Scalable and
backed by HAProxy.

**Note:**<br>
Due to the changes to links and networks, the haproxy currently needs to be removed
and recreated for link-changes (f.e. scale events) to be picked up.

## Deployment

1. Create a docker machine with your preferred driver
2. Connect your local Docker client to the machine `eval $(docker-machine env $name)`
3. Check the connectivity (`docker ps` should be usable and `docker-machine ls`
   should mark the machine active)
4. Deploy the app with `docker-compose up -d lb web`
5. Check the IP of the docker machine on port 80. It should show a nice `phpinfo` page
6. Check the IP of the docker machine on port 1936. Authenticate with `auth`/`auth`,
   you should see the haproxy page with one backend
7. Scale your app `docker-compose scale web=3`
8. Reload HAProxy by stopping, removing and restarting the container
   ```
   # Stop the balancer
   docker-compose stop lb
   # Remove the old balancer
   docker-compose rm lb
   # Bring the balancer back up
   docker-compose up --no-deps -d lb web
   ```

## License

MIT
