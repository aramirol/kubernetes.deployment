# kubernetes.deployment help file

## Ceph Dashboard

If you deploy this role with Rook Ceph Storage integrated, you can use the following command to show the `admin` password in order to use the web dashboard:

`kubectl -n rook-ceph get secret rook-ceph-dashboard-password -o jsonpath="{['data']['password']}" | base64 --decode && echo`

<p align="center">
  <img width="600" height="200" src="ceph_dashboard.png">
</p>

