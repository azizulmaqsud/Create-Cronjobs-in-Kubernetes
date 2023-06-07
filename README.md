# CronJobs in Kubernetes 
- How to Run Automated Tasks with a CronJob 

# Pre-requisite
- Kubernetes cluster
- kubectl command-line tool must be configured to communicate with your  cluster. 
- It is recommended to run a cluster with at least two nodes that are not acting as control plane hosts. If cluster not available, you can create by using minikube or you can use one of these Kubernetes playgrounds:

  -Killercoda https://killercoda.com/playgrounds/scenario/kubernetes
  -Play with Kubernetes https://labs.play-with-k8s.com/ 

# How to Create a CronJob
  Here is a sample config file for a CronJob to run task every minute: (Please rephrase it in Golang)

  -   apiVersion: batch/v1
  -   kind: CronJob
  -   metadata:
  -    name: hello
  -   spec:
  -    schedule: "* * * * *"
  -    jobTemplate:
  -     spec:
  -       template:
  -         spec:
  -           containers:
  -           - name: hello
  -             image: busybox:1.28
  -             imagePullPolicy: IfNotPresent
  -             command:
  -             - /bin/sh
  -             - -c
  -             - date; echo Hello from Kubernetes cluster
  -           restartPolicy: OnFailure

# Steps 

1. kubectl create -f https://k8s.io/examples/application/job/cronjob.yaml
2. cronjob.batch/hello created
3. kubectl get cronjob hello   (to get status)
4. The output looks like:

  - NAME        SCHEDULE        SUSPEND      ACTIVE      LAST      SCHEDULE      AGE
  - hello   */1 * * * *   False     0        <none>          10s

# Trouble Shooting
  If you can not see the cron job has not scheduled or run any jobs yet. Watch for the job after one minute

  kubectl get jobs --watch
  - The output:

  - NAME               COMPLETIONS   DURATION   AGE
  - hello-6222706355   0/1                      0s
  - hello-6222706355   0/1           0s         0s
  - hello-6222706355   1/1           8s         8s

Here one running job scheduled by the "hello" cron job. Now, you can stop then view the cron job again to see it scheduled the job:

  - kubectl get cronjob hello
  - The output:

  - NAME    SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
  - hello   */1 * * * *   False     0        55s             65s

Here 0 active jobs, meaning that the job has completed or failed.

# View pods
Now, you find the pods that the last scheduled job created and view the output of the pods.

Note: The job name is different from the pod name.
1. Replace "hello-6222706355" with your job name in system

  - pods=$(kubectl get pods --selector=job-name=hello-4111706356 --output=jsonpath={.items[*].metadata.name})
  - Show the pod log:
  - kubectl logs $pods
  
  - The output:

  - Thu Jun 06 10:05:89 UTC 2023
  - Hello from the Kubernetes cluster
  
# Deleting a CronJob
  kubectl delete cronjob hello

Note: Deleting the cron job removes all the jobs and pods; and, also, stops it from creating additional jobs. If needed then you can read more about removing jobs in garbage collection.

# Please share it and let's be connected with me always at the following
 
- https://www.youtube.com/channel/UCNwP7KEElaJ7cdDTLP-KbBg
- https://www.linkedin.com/in/azizul-maqsud/
- https://azizulmaqsud-1684501031000.hashnode.dev/
- https://twitter.com/Sohail2me
- https://www.buymeacoffee.com/azizulmaqsud


# Learn_IT_with_Azizul
# Creating-Cronjobs-in-Kubernetes
