# Set

```
LOADBALANCER=loadbalancer.yaml
PV=pv.yaml
PVC=pvc.yaml
PV_EFS=pv-efs.yaml
PVC_EFS=pvc-efs.yaml
JUPYTERLAB=jupyterlab.yaml
URL=https://raw.githubusercontent.com/biometrioearth/kubearth/main/deployments/no_cluster/
```

Next lines are not necessary but help to check last variables are set correctly:

```
wget $URL/$LOADBALANCER
wget $URL/$PV
wget $URL/$PVC
wget $URL/$PV_EFS
wget $URL/$PVC_EFS
wget $URL/$JUPYTERLAB
```

# Create service

```
kubectl create -f $URL/$LOADBALANCER
```

# Create storage

```
kubectl create -f $URL/$PV
kubectl create -f $URL/$PVC
kubectl create -f $URL/$PV_EFS
kubectl create -f $URL/$PVC_EFS
```
# Create Jupyterlab

According to selected instance. Download yaml:

```
wget $URL/$JUPYTERLAB
```

## If NO GPU

### Write docker image which will be in `jupyterlab.yaml`

For example:

```
sed -i "s/image: /image: biometrioearth\/dev:ecosystem_integrity_0.1/" $JUPYTERLAB
```

### Write CPU & memory

For example:

```
sed -i "s/cpu:/cpu: 0.5/" $JUPYTERLAB
sed -i "s/memory:/memory: 0.5Gi/" $JUPYTERLAB
```

## IF GPU

In addition of last writes in section **if no gpu**. Write:

```
sed -i '/limits:/a\                                          nvidia.com\/gpu: 1' $JUPYTERLAB
```

## Create deployment

After last writes

```
kubectl create -f $JUPYTERLAB
```
