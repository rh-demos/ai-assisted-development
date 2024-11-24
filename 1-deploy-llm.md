# Deploy LLM

Get deployment and configuration resource files

```shell
git clone https://github.com/rh-demos/llm-serving
```

![image-20241124160330371](assets/1-deploy-llm/image-20241124160330371.png)

## Install minio

Create a new project, named `minio`

![image-20241030163414180](assets/1-deploy-llm/image-20241030163414180.png)

Import minio deployment resources from the web console

![image-20241030163624146](assets/1-deploy-llm/image-20241030163624146.png)

The content of minio.yaml is as follows. You will need to adjust the root `username` and `password`.

https://github.com/rh-demos/llm-serving/blob/main/minio.yaml

```yaml
---
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: minio-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 400Gi
  volumeMode: Filesystem
---
kind: Secret
apiVersion: v1
metadata:
  name: minio-secret
stringData:
  # change the username and password to your own values.
  # ensure that the user is at least 3 characters long and the password at least 8
  minio_root_user: minio
  minio_root_password: your-password
---
kind: Deployment
apiVersion: apps/v1
metadata:
  name: minio
spec:
  replicas: 1
  selector:
    matchLabels:
      app: minio
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: minio
    spec:
      volumes:
        - name: data
          persistentVolumeClaim:
            claimName: minio-pvc
      containers:
        - resources:
            limits:
              cpu: 250m
              memory: 1Gi
            requests:
              cpu: 20m
              memory: 100Mi
          readinessProbe:
            tcpSocket:
              port: 9000
            initialDelaySeconds: 5
            timeoutSeconds: 1
            periodSeconds: 5
            successThreshold: 1
            failureThreshold: 3
          terminationMessagePath: /dev/termination-log
          name: minio
          livenessProbe:
            tcpSocket:
              port: 9000
            initialDelaySeconds: 30
            timeoutSeconds: 1
            periodSeconds: 5
            successThreshold: 1
            failureThreshold: 3
          env:
            - name: MINIO_ROOT_USER
              valueFrom:
                secretKeyRef:
                  name: minio-secret
                  key: minio_root_user
            - name: MINIO_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: minio-secret
                  key: minio_root_password
          ports:
            - containerPort: 9000
              protocol: TCP
            - containerPort: 9090
              protocol: TCP
          imagePullPolicy: IfNotPresent
          volumeMounts:
            - name: data
              mountPath: /data
              subPath: minio
          terminationMessagePolicy: File
          image: >-
            quay.io/minio/minio:RELEASE.2024-10-02T17-50-41Z
          args:
            - server
            - /data
            - --console-address
            - :9090
      restartPolicy: Always
      terminationGracePeriodSeconds: 30
      dnsPolicy: ClusterFirst
      securityContext: {}
      schedulerName: default-scheduler
  strategy:
    type: Recreate
  revisionHistoryLimit: 10
  progressDeadlineSeconds: 600
---
kind: Service
apiVersion: v1
metadata:
  name: minio-service
spec:
  ipFamilies:
    - IPv4
  ports:
    - name: api
      protocol: TCP
      port: 9000
      targetPort: 9000
    - name: ui
      protocol: TCP
      port: 9090
      targetPort: 9090
  internalTrafficPolicy: Cluster
  type: ClusterIP
  ipFamilyPolicy: SingleStack
  sessionAffinity: None
  selector:
    app: minio
---
kind: Route
apiVersion: route.openshift.io/v1
metadata:
  name: minio-api
spec:
  to:
    kind: Service
    name: minio-service
    weight: 100
  port:
    targetPort: api
  wildcardPolicy: None
  tls:
    termination: edge
    insecureEdgeTerminationPolicy: Redirect
---
kind: Route
apiVersion: route.openshift.io/v1
metadata:
  name: minio-ui
spec:
  to:
    kind: Service
    name: minio-service
    weight: 100
  port:
    targetPort: ui
  wildcardPolicy: None
  tls:
    termination: edge
    insecureEdgeTerminationPolicy: Redirect
```

After the creation is complete, access the minio console and create a new bucket named `models`.

![image-20241030164213228](assets/1-deploy-llm/image-20241030164213228.png)

Create a new AKSK, download and save it for later usage.

![image-20241030164656987](assets/1-deploy-llm/image-20241030164656987.png)

## Deploy model

Navigate to OpenShift AI web console page from quick link.

![image-20241030164939257](assets/1-deploy-llm/image-20241030164939257.png)

Add a custom notebook image for downloading large model.

- Image location: `quay.io/jonkey/rhods/odh-pytorch-notebook-aria2:2023.2-2`
- Notebook name: `odh-pytorch-notebook-aria2`

![image-20241030165518164](assets/1-deploy-llm/image-20241030165518164.png)

Create an AI project named `llm`

![image-20241124160630701](assets/1-deploy-llm/image-20241124160630701.png)

Create a workbench in llama project with the following information:

- Name: `my-wb`
- Notebook Image: `odh-pytorch-notebook-aria2`

![image-20241124160723845](assets/1-deploy-llm/image-20241124160723845.png)

Environment

-  HF_USERNAME: `<your huggingface username>`
-  HF_TOKEN: `<your huggingface token>`

![image-20241030171820997](assets/1-deploy-llm/image-20241030171820997.png)

- Container size: `small`
- Storage name: `my-wb`
- Storage size: `20G`

![image-20241124160832194](assets/1-deploy-llm/image-20241124160832194.png)

- Data connection name: `models`
- Data connection AK: `<your SK>`
- Data connection SK: `<your SK>`
- Data connection Endpoint: `<your SK>`
- Data connection Region: `none`
- Data connection Bucket: `models`

![image-20241030171951914](assets/1-deploy-llm/image-20241030171951914.png)

Waiting for workbench creation and running, click `Open` link navigate to the created workbench.

![image-20241124160954384](assets/1-deploy-llm/image-20241124160954384.png)

Git clone source code, the url is: https://github.com/rh-demos/llm-serving

![image-20241124161201032](assets/1-deploy-llm/image-20241124161201032.png)

Run the download-mode-to-s3 notebook to download the model.

![image-20241124161257146](assets/1-deploy-llm/image-20241124161257146.png)

Save the model to object storage.

![image-20241124161340826](assets/1-deploy-llm/image-20241124161340826.png)

Log in to minio and view the uploaded models.

![image-20241124161517834](assets/1-deploy-llm/image-20241124161517834.png)

Deploy the model according to the following information

- Model name: `granite`
- Serving runtime: `vLLM ServingRuntime for KServe`
- Model frame work: `vLLM`
- Model server replicas: `1`

![image-20241124162011896](assets/1-deploy-llm/image-20241124162011896.png)

- CPUs requested: `2`
- CPU limit: `4`
- Memory requested: `8Gi`
- Memory limit: `16Gi`
- Accelerator: `NV GPU`
- Number of accelerators: 1
- Make deployed models available through an external route: `checked`
- Require token authentication: `checked`

![image-20241124162044081](assets/1-deploy-llm/image-20241124162044081.png)

Service account name: `default-name`

Existing data connection:

-  Name: `models`
- Path: `models/Llama-3.1-70B-Instruct`

![image-20241124162115288](assets/1-deploy-llm/image-20241124162115288.png)

Wait for the deployment to complete.

![image-20241124162200839](assets/1-deploy-llm/image-20241124162200839.png)

Get and save the url and token from the running model

- Inference endpoint: `<your Inference endpoint >`
- Token secret: `<your Token secret >`

