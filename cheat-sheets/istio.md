- Make the Mutual TLS permissive fo the ``foo`` namespace

  ````yaml
  cat <<EOF | kubectl apply -f -
  apiVersion: "authentication.istio.io/v1alpha1"
  kind: "Policy"
  metadata:
    name: "default"
    namespace: "foo"
  spec:
    peers:
    - mtls: 
       mode: PERMISSIVE
  EOF
  ````
