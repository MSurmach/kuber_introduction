# Instructions for connecting to a Kubernetes cluster 
### **Before you begin:**
> 1.  Install and set up **`kubectl`** using this link: https://kubernetes.io/docs/tasks/tools/install-kubectl-windows <br/>

**Important note:** You must use a **`kubectl`** version that is within one minor version difference of your cluster. For example, a v1.27 client can communicate with v1.26, v1.27, and v1.28 control planes. Using the latest compatible version of **`kubectl`** helps avoid unforeseen issues.
> 2. Test to ensure the version of **`kubectl`** is the same as downloaded: </br>
```console
> kubectl version --output=json
{
  "clientVersion": {
    "major": "1",
    "minor": "25",
    "gitVersion": "v1.25.9",
    "gitCommit": "a1a87a0a2bcd605820920c6b0e618a8ab7d117d4",
    "gitTreeState": "clean",
    "buildDate": "2023-04-12T12:16:51Z",
    "goVersion": "go1.19.8",
    "compiler": "gc",
    "platform": "windows/amd64"
  },
  "kustomizeVersion": "v4.5.7",
  "serverVersion": {
    "major": "1",
    "minor": "25",
    "gitVersion": "v1.25.9",
    "gitCommit": "a1a87a0a2bcd605820920c6b0e618a8ab7d117d4",
    "gitTreeState": "clean",
    "buildDate": "2023-04-12T12:08:36Z",
    "goVersion": "go1.19.8",
    "compiler": "gc",
    "platform": "linux/amd64"
  }
}
```
> 3. **`Docker Desktop`** makes developing applications for Kubernetes easy. It provides a smooth Kubernetes setup experience by hiding the complexity of the installation and wiring with the host.</br>

**Note:** Please ensure, that you have it installed. Or you may install it using this link: https://docs.docker.com/desktop/install/windows-install

> 4. Kubernetes can be enabled from the Kubernetes settings panel as shown below:
![img.png](img.png) <br/> 
Checking the Enable Kubernetes box and then pressing Apply & Restart triggers the installation of a single-node Kubernetes cluster. </br>
After this step we have a default context and cluster provided by Docker. We can check it running kubectl commands. </br>
```console
> kubectl config current-context
docker-desktop
```
```console 
> kubectl config get-clusters
NAME
docker-desktop
```

## Configure access to Godel Technologies Cluster

At this step we have the default Kubernetes cluster provided by Docker, and the kubectl command-line tool configured to communicate with this cluster. We can quickly switch between clusters by using the `kubectl config use-context` command (here is a link if you need an additional information https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters).

### Configuration file
To connect to Godel's cluster we will use the config as shown below.
```yaml
apiVersion: v1
clusters:
  - cluster:
      certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUMvakNDQWVhZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJek1ESXdOekV5TlRFek1sb1hEVE16TURJd05ERXlOVEV6TWxvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTGdUCjF1UnNzWTZ4Q1F6OXZiK2c1cmc1NC91MHNZVFhMeTFsQ3lQVU1pczFnVGs4bnhWM3pqYjdnNFRzb2p4ZFpPRVUKWm8rZFA2blQ0S3FCRjJuZHBEUEJjYlgwcEFTSDBrVlVSS3k4bUFac0k5UXd2ZkxTZWdyRjI1RDNwWkJ6MzNFSgpKQ3JEakxvVXIzQk0vd0hhSkVPbmNDRE9OM2h6cVRlM3lncHJyMmhLcmRkNkNWVkdQcDBNaHYyOUgzNmhiQXVFCndCUjNGYTQwOGVyTXJSSFlsN0dzQlZGL01pZlMxRmlyL0JTdmJEQzhHUE1tOG9UVERMWUtlR0hiQ0NoMFBBVGYKZUI4VFN3ejRwUm0vSHBZR3lCKzJOc3JvTVd0RFh6SkJxempFWXZQUFBQa01ZbE1FUUI1SHZTbnd3OTVuWkVkagpPNFQ4TmZ5VVdVSnFHRUdHZGwwQ0F3RUFBYU5aTUZjd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0hRWURWUjBPQkJZRUZIbXozZFFlcDhncCtDZVhUQVB4b3VZNnJRNlpNQlVHQTFVZEVRUU8KTUF5Q0NtdDFZbVZ5Ym1WMFpYTXdEUVlKS29aSWh2Y05BUUVMQlFBRGdnRUJBSGx4eVZKZDV4bG5KdGtCNWpESApOMU80MTBrdkxYTjkza3VjVWJTaDhrN2M3OXlWOXBJbzBvV2xJbEM4RmdOcmZJSzRvNFhKRytuL3l6b0diT3B0Cko1YlN4SFJDeGc5YlZ3eVYwcUJJUG03QXJNN3FtYUhXd3VzQWUyZHRLeVJ0Qk03aTBFOG1idkZoMENTVW5vdW8KS0FSMTJBVXZLeCsrL2RyMkRVQTRwWTVFT0ZTbzhXUEFjUnVJUU92M2F4U3ZrUVc1NjQxa2VtdGJNbzUycFRwNgo4dHJwZ1MvcDRHN3B5akF0c0d0Vk80RFNZdnVqZnZURXZGS0dycXNyQkthWnd2Tm1LekJDamNaak92cGg3OXBTClh4ZHAxQm9EMEtlOXBKM0tHTGNSeGJLZ003K3ViR1d3VFBNZ3VPZ2QyWG5STGcvZDJ2c0RPUnQ1dzh4cGtZVjAKVlB3PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
      server: https://192.168.80.98:6443
    name: kubernetes
contexts:
  - context:
      cluster: kubernetes
      namespace: java # namespace name
      user: developer-java # user name
    name: developer-java@kubernetes
current-context: developer-java@kubernetes
kind: Config
preferences: {}
users:
  - name: developer-java
    user:
      client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUREakNDQWZhZ0F3SUJBZ0lRRlR5ZHJrSDB6MVZyUEYzQk00S2tFakFOQmdrcWhraUc5dzBCQVFzRkFEQVYKTVJNd0VRWURWUVFERXdwcmRXSmxjbTVsZEdWek1CNFhEVEl6TURVeU1qSXdNell4TmxvWERUSTBNRFV5TVRJdwpNell4Tmxvd0dURVhNQlVHQTFVRUF4TU9aR1YyWld4dmNHVnlMV3BoZG1Fd2dnRWlNQTBHQ1NxR1NJYjNEUUVCCkFRVUFBNElCRHdBd2dnRUtBb0lCQVFETFZEMTdmbzIzVnVYdTRzSklVWElMbDhtL3cyaXNqRUZnd01TcU9ZS2MKNWpsSWE1a2hzdG9xVFhEaWNTcTZwT3hZY2ZnQkthUmZLbEpmTWQrUEFkN3Q1ejlxZVZaV2RlWmREQTJFQkd6QgpJbWVIQTJXU2hBZVhNeTcwYnVuY3lSbFRwT2RGWlU1TjZOZGVVeS81V09Cbld0NjBFem9CRG5ncFQ1M2NVRjBwCk1JNktqclpSWmNleTd1d1ZHWG5tNzRVK0x1RFd3Ynh4ek1jSVdjNWJGV3ltU2dhUHBpNjdKa3ZJQUFWMkR2TzIKVjJCcyt3L2VSdUdFZlo3NnFlaG5EekRkbzBXVXM2NnJtWjg1dE5GbEYvTWlpUjVya2dpaUpsSFBHNGpBL3YyMwo3T0MxRUJMQXU2TElOMStkQjg4YU9HMURqTFRQWGViS1oxRHdITFVTblcrZEFnTUJBQUdqVmpCVU1BNEdBMVVkCkR3RUIvd1FFQXdJRm9EQVRCZ05WSFNVRUREQUtCZ2dyQmdFRkJRY0RBakFNQmdOVkhSTUJBZjhFQWpBQU1COEcKQTFVZEl3UVlNQmFBRkhtejNkUWVwOGdwK0NlWFRBUHhvdVk2clE2Wk1BMEdDU3FHU0liM0RRRUJDd1VBQTRJQgpBUUFnaUhRemFkbXBxeVd1UGhsbDFBR2V2ZGtGN2tTL3RtOWZ2bkMyK0VuNmYzRmVORUp3dzl6RS9leGlYNFZTCnBGYVFKSjROMHYwZEo4eXozcHd4MnFiRThJdjJzVXc2UkxLR1hxOEdVNlRjUUlVRzF0d01WRHNYaExGTHBTOG4KNzRRbDVSY3M3bldKSGtUNWtoRVpNRWFDTDRFU3FmaE5qMUNLc0ZDSGpidnRFL0FWSy9yNVRwWUl2OTdva1owQgp3L1luT1JkQUFScDlzeTVSeDFrSzVpaU1KbXg5Wk13SllkZmo2R3RQeHhRRVBuNXM2Y1BvRjVDYXhRU0pWdUphCm1Yd3FXSnpwUEdzMG9PN0svRkU0RUxCNkVTckoyZm5EaVVGSjM1blhuaVJEcHZ0dnBDNWRaelVRTUF0RFlOTnoKWmpMYmwxQXBmbi9HbVU5RWR0VVVra1VjCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
      client-key-data: LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFb2dJQkFBS0NBUUVBeTFROWUzNk50MWJsN3VMQ1NGRnlDNWZKdjhOb3JJeEJZTURFcWptQ25PWTVTR3VaCkliTGFLazF3NG5FcXVxVHNXSEg0QVNta1h5cFNYekhmandIZTdlYy9hbmxXVm5YbVhRd05oQVJzd1NKbmh3TmwKa29RSGx6TXU5RzdwM01rWlU2VG5SV1ZPVGVqWFhsTXYrVmpnWjFyZXRCTTZBUTU0S1UrZDNGQmRLVENPaW82MgpVV1hIc3U3c0ZSbDU1dStGUGk3ZzFzRzhjY3pIQ0ZuT1d4VnNwa29HajZZdXV5Wkx5QUFGZGc3enRsZGdiUHNQCjNrYmhoSDJlK3Fub1p3OHczYU5GbExPdXE1bWZPYlRSWlJmeklva2VhNUlJb2laUnp4dUl3UDc5dCt6Z3RSQVMKd0x1aXlEZGZuUWZQR2podFE0eTB6MTNteW1kUThCeTFFcDF2blFJREFRQUJBb0lCQUZScHZMeXdaWlZmOWtXZQp6cG5IZGxscHd0QmlCK3JhcjZuRFhlSnR6ZFBsb0pKNFdUS3NWZmFKLy91Q2tBSzh4WUpTam11dEpoaDhNWVpqCjVqUXd5cVJxQk9IblRmakhLY1FuWk5VU0lUUnRYQjJwUTFuNGhrNDNhWjhCRFFZa1Z1ZHE1cmpndmdtS1NSOVgKMmVyakF3YmxxdCtIdStVRVpNNkJ4ei9YL1ZWRG5UMXZXeEoxODV4cCtiMkUzMzV2TGVsWlRQL3ljTHc5Y3NQegpEOUp3c2Z3T2tOenlZanVRNGx2K3I3bHRMK05sOW9kM081QVFqdGw4Rnk4dHYrSVVUZzg2NngvMlhXenZ0VnBQCjhmSkx0TjRNem1aNVpQSnNSSmJMRlZvNjVDby9vZjk1b0tGWEhSaTJxSndIc1lJaEJDVTN0cjFOVHRMQ2NER3IKd2lwQmIrRUNnWUVBK1NucUJ2WVM4cXdWWXcwelZXQWNVZGRVYXdmWXk3SjRORUZ0OURsSUhjK3FjREJEaDRjdQorS0N5cW1XSHROeXQxa0xoMTlSWkdoc2FmamhWeEFweWR0akFLYnFEN3dTWXlGLzI1YXZPNjJhcXVqeWNka3BoCmR0KzUrL3VpSFlTU05oL1dXQll3a2tOaC9NUjZlcUk4djRBdDFQTXI5dHA5OG5ISEUrQUJ0ZGNDZ1lFQTBPaGsKQllhcnE5U3FnZlhGZ0xRbXRqZ3hNR1ZQQUxlcmNsSllieDVvN2NTZzBoc0NBdnFVWEplRjJaaC9vWE4yai93TgptVGkxajUyc2I4T1AxMjkzVUs0cUpMVVFPL2g1ZW94UCs4K1dmaklIckgwck5WaVY1UkZvTmNnbFE4LzFuSVAwClFJd1BlNDZvcVlCQlZCTSsxbTA1SUQ3cDhEelQ0NGtpNzMvTnI2c0NnWUEwRERkZ3dPSndZdFlNM09NT1FJZHAKNlNzdk9ISm5DcDdsZTQxMmFNalJ3V0YvRWZYcFI2bmVNZU5naU5qeVJPN2pSUkZjUG5WbWFLYitHdEovb3hjSwpFSGtEdm54MDMyYnk1R2owelJEcVI0WCtaVTdodnVQaEt4YjB0am5GSkw1T0pCUm9UaXBUUTFRaXN3Qkt3YjBpCjI2bTNSQnk0OGlkQkQ5ZGdKWGoxL1FLQmdERXhCNXhsZ2JjTWRMbjFiWFJJOTNTZzlyUTJwc3U4ZEJxUFdWT1IKRFpRdllNOFluUGltbUpqVTlTZXZ5UjN6V2srOVFsWjZEQWF0VWhWZkwxZkRYNDBYOWNSKy9qdWczcm9Fb0REagpTTVNlYzBBaW1USU9rZjFHaEF0bC9hQmlMNnVUUnNpTU1ua2R0b2x6TWU3aU9VRzBwbWI1YVl5d1JoU21EaHZuCk5EL1JBb0dBZHJUbEdMY21sNkE2MHcrVjlkSHVnWUVtMU1aZ29wOHJCeWZWMlZjbFlUQjJOVzdjMlJTVktJWCsKbjZxT2ZkTjVkMGNwUHM3RWRZamRUQUZFek9PSnZ0U3h6elI3MlZMUWZpS0wzRDFZSXo3SzExTU1jRjh1aktRUQp4d1BPcGtmZmlkZHc3Q0orRFI1Mjd3d2ttK0EvN2dDVmh0TTlJbXUrcUxYTUREekg0YWc9Ci0tLS0tRU5EIFJTQSBQUklWQVRFIEtFWS0tLS0tCg==
```

**Note:** A file that is used to configure access to a cluster is sometimes called a **kubeconfig file**. This is a generic way of referring to configuration files. It does not mean that there is a file named `kubeconfig`. But, in our case, please save this config on your local computer as a file and name it as `kubeconfig` without any extension.

### Set the KUBECONFIG environment variable
See whether you have an environment variable named `KUBECONFIG`.</br>
The `KUBECONFIG` environment variable is a list of paths to configuration files. For Windows, the list is semicolon-delimited. If the `KUBECONFIG` environment variable does exist, `kubectl` uses an effective configuration that is the result of merging the files listed in the `KUBECONFIG` environment variable.</br>

If so, save the current value of your `KUBECONFIG` environment variable, so you can restore it later. For example:
```console
> $Env:KUBECONFIG_SAVED=$ENV:KUBECONFIG
```
Add our `kubeconfig` file to the `KUBECONFIG` environment variable as shown below.
```console
> $Env:KUBECONFIG=("<path-to-file>/kubeconfig")
```
To see your configuration, enter this command:
```console
> kubectl config view
```
And the output will look something like this:
```yaml
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://192.168.80.98:6443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    namespace: java
    user: developer-java
  name: developer-java@kubernetes
current-context: developer-java@kubernetes
kind: Config
preferences: {}
users:
- name: developer-java
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
```

And finally we can check the cluster:
```console
> kubectl cluster-info --namespace="java"
Kubernetes control plane is running at https://192.168.80.98:6443
```
The namespace:
```console
> kubectl get namespace java -o json
{
    "apiVersion": "v1",
    "kind": "Namespace",
    "metadata": {
        "creationTimestamp": "2023-05-22T20:16:52Z",
        "labels": {
            "kubernetes.io/metadata.name": "java"
        },
        "name": "java",
        "resourceVersion": "13811433",
        "uid": "5547353b-4dd9-4aa8-bdbc-72648d5175c4"
    },
    "spec": {
        "finalizers": [
            "kubernetes"
        ]
    },
    "status": {
        "phase": "Active"
    }
}
```

## We can now run `kubectl` commands and deploy applications.
