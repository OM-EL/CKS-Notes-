# CKS-Notes-
Notes i take when preparing the CKS

## AppArmor ( Kub > 1.4 ) :

1. Load the AppArmor profile :
    >  apparmor_parser -q /etc/apparmor.d/frontend
2. Check that that the profile have being correctly loaded  :
    >   aa-status | grep profile-name
3. To see the available appArmor profiles (preloaded in the kernel):
    > Check : /sys/kernel/security/apparmor/profiles
4. Apply AppArmour on profile :
    > apiVersion: v1
      kind: Pod
      metadata:
        name: hello-apparmor
        annotations:
          # Tell Kubernetes to apply the AppArmor profile "k8s-apparmor-example-deny-write".
          # Note that this is ignored if the Kubernetes node is not running version 1.4 or greater.
          container.apparmor.security.beta.kubernetes.io/hello: localhost/k8s-apparmor-example-deny-write
      spec:
        containers:
        - name: hello
          image: busybox:1.28
          command: [ "sh", "-c", "echo 'Hello AppArmor!' && sleep 1h" ]
## Practical inforamtion :
1. Add this to anything you want to deccode :
    >  | base64 --decode
2. When making Network policy and you add a PodSelect it selects pods from the same namesapce that the network policy is inside.
3. grep can not only be called on a cat .. after a pipe , it can also be called on files : 
    > grep -r "Package management process launched" .
4. to make an ingress make https certificat , you create a secret out of the key and certificat , and you add a part under tls to pay certificat.
5. **crictl**
     >  crictl ps -id containerId
     > 

## Trivy Analysis :
1. The command to analyse a given image :
     > trivy image --severity CRITICAL kodekloud/webapp-delayed-start

   
## SecComp  Analysis :
1. Profile need to be at “/var/lib/kubelet/seccomp/profiles”
2. The Pod's Yaml looks should look like this :
    > spec:
        securityContext:
          seccompProfile:
            type: Localhost
            localhostProfile: profiles/audit.json
        containers:
        - image: nginx
          name: nginx
    
## RuntimeClass :
1. runtimeClassName :
    > spec:
        runtimeClassName: gvisor
        containers:
            -
                image: nginx
                name: busy-rx100
## Falco :
1. Falco configuration file path : " /etc/falco"
2. Falco generated events :
    > journalctl -fu falco
