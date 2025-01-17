# Coinflow Frontend

This is a frontend visualization of the Dataflow/Bigtable/Coinbase example.

This is based on the simpler [Managed VMS example](https://github.com/GoogleCloudPlatform/cloud-bigtable-examples/tree/master/java/managed-vm-gae).

# Prerequisites

1. Follow all the steps to start the Dataflow/Bigtable backend pipeline
1. Install [Bower](http://bower.io/)
1. `bower install`

## Deploying the AppEngine Runtime
1. get a copy of the [appengine-java-vm-runtime](https://github.com/GoogleCloudPlatform/appengine-java-vm-runtime/tree/jetty-9.2]

1. Make sure branch jetty-9.2 is checked out

1. Follow the instructions to build the docker container

1. Substitute your ProjectID for PROJECT_ID_HERE, assuming you ran `./buildimage.sh myimage` in
the appengine-java-vm-runtime repo:

  * `docker tag myimage gcr.io/PROJECT_ID_HERE/gae-mvm-01`
  * `gcloud docker push gcr.io/PROJECT_ID_HERE/gae-mvm-01`
  * `gcloud docker pull gcr.io/PROJECT_ID_HERE/gae-mvm-01`
<!-- The gcloud docker pull may not be required, but it made life easier -->

1. Edit `Dockerfile` to set `PROJECT_ID_HERE` in the **FROM** directive, `BIGTABLE_PROJECT`, `BIGTABLE_CLUSTER`, and `BIGTABLE_ZONE` (if necessary)

1. Build the java artifacts and docker image

    `mvn clean compile process-resources war:exploded`<br />
    **Note** - you can use `mvn package` but you'll get an ignorable error on the next step.

# Updating the Table Name

If your table is not name 'coinbase', make sure you update the TABLE name in CoinflowServlet.java.

# To Run Locally

1. Add your Service Account JSON credentials to src/main/webapp/WEB-INF
1. Uncomment the line in src/main/webapp/Dockerfile that points the GOOGLE_APPLICATION_CREDENTIALS
to that file.
1. Then run:

    `mvn gcloud:run`

Note: if you have Docker or boot2docker installed locally, changing the gcloud maven configuration
in the pom file from 'remote' to 'local' may speed up your container build.

## Local Debugging Tips

* When you run `mvn gcloud:run`, make sure port 8000 and port 8080 are available
* Use `docker ps` to look for previous containers running the same app, and `docker stop` to stop
 them.
* You can also use `docker logs` to inspect the state of the container and see any error logs in
the Jetty app.

# To Deploy

1. Make sure the GOOGLE_APPLICATION_CREDENTIALS line in src/main/webapp/Dockerfile is commented out.
1. Run:

    `mvn gcloud:deploy`

Copyright Google 2015
