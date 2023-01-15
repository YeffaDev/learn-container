# Restart Policy

## Task 1 - Always Restart

The policy restart a terminating POD even if it exit succesfully with ReturnCode=0

- [AlwaysRestartPod.yaml](https://github.com/YeffaDev/learn-kubernetes-brownbag/blob/master/lab/yaml/06/AlwaysRestartPod.yaml)

```
vi AlwaysRestartPod.yaml
kubectl get po --watch &
kubectl apply -f AlwaysRestartPod.yaml      # The POD stay live for 5 sec, then exit succesfully
fg
ctrl+c
kubectl describe pod demopod1 | grep Exit
kubectl delete -f AlwaysRestartPod.yaml --force --grace-period=0
```

## Task 2 - OnFailure Restart

The policy restart a terminating POD only when it fails (exit code=1)

- [OnFailureRestartPod.yaml](https://github.com/YeffaDev/learn-kubernetes-brownbag/blob/master/lab/yaml/06/OnFailureRestartPod.yaml)

```
vi OnFailureRestartPod.yaml
kubectl get po --watch &
kubectl apply -f OnFailureRestartPod.yaml        # The POD stay live for 5 sec, then it fails
fg
ctrl+c
kubectl describe pod demopod2 | grep Exit
kubectl delete -f OnFailureRestartPod.yaml --force --grace-period=0
```


## Task 3 - Never Restart

The policy never restart a terminating POD.

- [NeverRestartPod.yaml](https://github.com/YeffaDev/learn-kubernetes-brownbag/blob/master/lab/yaml/06/NeverRestartPod.yaml)

```
vi NeverRestartPod.yaml
kubectl get po --watch &
kubectl apply -f NeverRestartPod.yaml        # The RESTARTS field stay 0
fg
ctrl+c
kubectl describe pod demopod3 | grep Exit
kubectl delete -f NeverRestartPod.yaml --force --grace-period=0
```
