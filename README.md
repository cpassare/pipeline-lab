# pipeline-lab
Laboratory to try Openshift Pipelines

## Steps to prepare

1. Clone this repository

```
 $ git clone https://github.com/cpassare/pipeline-lab
```

2. Dowload tkn CLI

```
 $ curl -so tkn.tar.gz https://mirror.openshift.com/pub/openshift-v4/clients/pipeline/0.9.0/tkn-linux-amd64-0.9.0.tar.gz && gunzip tkn.tar.gz && tar xf tkn.tar
```

3. Create a simple application

```
 $ oc new-project lab-tekton
 $ oc create -f assets/sample_app.yaml
 $ oc new-app centos/mongodb-36-centos7 -e MONGODB_USER=admin MONGODB_DATABASE=mongodb MONGODB_PASSWORD=secret MONGODB_ADMIN_PASSWORD=super-secret
 $ oc set env dc/nodejs-ex MONGO_URL="mongodb://admin:secret@mongodb-36-centos7:27017/mongodb"
```

4. Install Openshift Pipelines Operator

```
 $ oc create -f assets/openshift_pipeline_subscription.yaml
 ## Check if is correctly installed
 $ oc api-resources --api-group=tekton.dev
```
5. Create tasks

```
 $ oc apply -f assets/s2i-nodejs-task.yaml
 $ oc apply -f assets/openshift-client-task.yaml
```
6. Create pipeline

```
 $ oc create -f assets/deploy-pipeline.yaml
```

7. Create pipeline resources

```
 $ oc create -f assets/git-pipeline-resource.yaml
 $ oc create -f assets/image-pipeline-resource.yaml
```


## Check resources
```
 $ tkn task ls 
 $ tkn pipeline ls
 $ tkn resource ls
 $ tkn resource describe nodejs-ex-git
```

## Run the pipeline
```
 $ tkn pipeline start deploy-pipeline -r app-git=nodejs-ex-git -r app-image=nodejs-ex-image -s pipeline
```
## Check the pipeline run
```
 $ tkn pr ls
```


